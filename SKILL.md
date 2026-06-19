---
name: daily-collector
description: Builds the structured daily log. Reads yesterday's Fellow transcripts from the repo (fellow/{date}/), pulls Outlook, Zoom, Jira, Confluence, and Slack from live connectors, normalizes everything into the daily-log schema, tags entities, and commits one file. Invoked by the daily-note Cloud Routine.
---

# daily-collector

Invoked by `routines/daily-note-prompt.md` (inside a subagent, so connectors
initialize). Read `config.yaml` and `schema/daily-log-schema.md` first.

## Step 1: window

From `config.yaml`, with `day_boundary.mode: previous`, the window is yesterday
00:00:00 to 23:59:59 in `identity.timezone`. Hold local and UTC bounds. `date` in
the output is that local day. Call it DATE.

## Step 2: capture each source

Wrap every source in its own error boundary; on failure add it to
`sources_failed` and a one-line reason to `capture_notes`, then continue. Order
below; Slack last.

### Outlook (Microsoft 365) is the spine

- Calendar: `outlook_calendar_search` for events starting in the window. Each
  becomes a meeting candidate: title, start-end, attendees.
- Email: `outlook_email_search` for mail in the window. Apply the comms filter
  (Step 3). Kept items go to `comms_highlights`; lift any explicit commitment or
  decision into `action_items` / `decisions`.

### Fellow (read from the repo, NOT the connector)

The fellow-dump routine already wrote yesterday's meetings to `fellow/DATE/` and
committed them to the branch you cloned. Do not call the Fellow connector.

- List files under `fellow/YEAR/DATE/` (where YEAR is the four-digit year of DATE). Each is one meeting in the fellow-dump schema:
  frontmatter (`title`, `time`, `participants`, `fellow_url`) plus `## AI Summary`
  (or `## AI Notes`) and `## Transcript` sections.
- Parse each file fully. Extract:
  - **Frontmatter**: title, time, participants, fellow_url — used for the
    meetings index line and meeting_notes block header.
  - **AI Summary**: the full AI-generated summary text, including all chapter
    headings and their bullet points with timestamps. Copy verbatim; do not
    paraphrase or shorten. This is the primary source for `meeting_notes`.
  - **AI-Generated Action Items** and **Decisions** subsections within the AI
    Summary: extract these for `action_items` and `decisions` sections.
  - **Transcript**: read the transcript when the AI Summary is missing or sparse.
    Extract key moments, decisions, and commitments as timestamped bullets for
    `meeting_notes`. You do not need to reproduce the full transcript, but do
    capture all substantive content — topics discussed, conclusions reached,
    open questions, and anything that would inform a downstream weekly summary.
- For the **meetings index line**: derive the one-clause `outcome` from the
  AI Summary's Decisions or the top-level summary paragraph.
- For the **meeting_notes block**: write the full AI summary verbatim, then
  all chapter bullet points (with [HH:MM:SS] timestamps), then Decisions and
  Actions subsections. If no AI summary exists, use the transcript to produce a
  detailed set of discussion bullets covering every topic raised.
- Record the file path (e.g. `fellow/2026/2026-06-17/de-1-1-kapil.md`) as the
  meeting's `notes:` value in the meetings index line.
- If `fellow/YEAR/DATE/` is missing or empty: add `fellow: no transcripts in repo
  for DATE` to `capture_notes` and continue. Not a hard failure. Keep
  `fellow_source: repo` in the frontmatter.

### Zoom (connector)

- `recordings_list` or `search_meetings` over the window; `get_meeting_assets`
  for AI summary, recording URL, action items. Carry the asset URL for merge.

### Merge meetings (before rendering)

The Outlook calendar event is the canonical meeting object. Attach the Fellow
file (from the repo) and the Zoom assets to the event they belong to.

- Match key: normalized title (lowercased, trimmed) AND start within +/- 15 min.
- A Fellow or Zoom meeting with no calendar match is its own meeting object.
- Never emit one meeting as two lines.

### Jira (Atlassian)

1. Candidates via `searchJiraIssuesUsingJql` (previous-day):
   ```
   (project in (DE) OR assignee = currentUser() OR reporter = currentUser())
   AND updated >= startOfDay(-1) AND updated < startOfDay()
   ```
   Substitute projects from `jira.project_keys`.
2. `getJiraIssue` with `fields: ["*all"]`, changelog and comments expanded.
   Filter to entries authored by the identity in the window.
3. One `ticket_activity` line per discrete action: transition (`From -> To`),
   comment (gist), worklog (duration), or `created`.
4. Also catch worklogs directly:
   `worklogAuthor = currentUser() AND worklogDate = "DATE"`.

### Confluence (Atlassian)

- `searchConfluenceUsingCql`, prioritizing `confluence.space_keys`:
  ```
  contributor = currentUser()
  AND lastmodified >= "DATE 00:00" AND lastmodified < "DATE+1 00:00"
  AND type in (page, blogpost)
  ```
  Parallel query with `creator = currentUser()` to flag created vs edited.
- For each page found, call `getConfluencePage` to retrieve the page body.
  Extract a detail description covering: what the page is about, what major
  sections or content it contains, and — where discernible from the structure
  or content — what was likely added or updated in this session (e.g. new
  sections, tables filled in, decisions appended, draft status changed). For
  created pages, summarize the full scope of what was written. For edited pages,
  describe what the page covers and note any sections that appear newly added
  or substantially changed. Keep the detail to 1–3 factual clauses; do not
  quote large blocks of page text.

### Slack (last, most fragile)

- **DMs**: search for all DM conversations active in the window using
  `slack_search_public_and_private` with `from:<@USERID>` and also search for
  messages sent TO the identity. For every DM thread that had activity, call
  `slack_read_thread` to get the full thread context — do not capture just the
  search-result snippet. A DM thread counts as one `comms_highlights` entry; the
  detail field should summarize the full exchange (what was asked, what was
  answered, what was decided or unresolved), not just the triggering message.
- **Mentions**: `slack_search_public_and_private` for messages mentioning the
  identity in any channel. Read the thread for each hit.
- **Allowlisted channels** (`slack.channel_allowlist`): `slack_read_channel` over
  the window; capture every message the identity sent and every thread they
  participated in; `slack_read_thread` for full thread context.
- Never read `slack.channel_blocklist`.
- On rate limit, keep what you have, note `slack: rate limited, partial capture`,
  and only mark failed if you got nothing.

## Step 3: the comms filter (Slack + email)

**Slack DMs**: Keep every DM thread where real work was discussed — questions,
status updates, blockers, technical details, decisions, follow-ups. Do not drop
a thread just because it also contains small talk; capture the work content.
Only drop threads that are entirely social with zero work content.

**Slack channels / mentions**: Keep messages where the identity sent or received
a substantive message — a question, an answer, a direction, a status update, a
blocker, a technical clarification. Drop pure acknowledgements ("👍", "thanks",
"got it") when they stand alone with no content.

**Email**: Keep emails that contain a decision, direction, ask, commitment, or
status change on real work. Drop automated notifications, calendar confirmations,
and pure logistics. When a PR review email has substantive feedback content,
keep it.

For any kept item that also carries an action or decision, lift it into
`action_items` / `decisions` as well. Err strongly toward keeping; this is
bronze and the downstream agents need the raw material.

## Step 4: normalize and tag entities

Over every captured text block: tickets via `entities.patterns.ticket`, page ids
via `entities.patterns.confluence_page_id`, people via case-insensitive `roster`
match (emit canonical name), keywords via verbatim `entities.keywords` match.
Fill per-line `entities:` fields and build the frontmatter `entities` union
(deduped, sorted).

## Step 5: draft update sections

Using all captured data from Steps 1–4 as source material, draft the three
standing update sections. These are written for a human to review and post;
be specific and factual, draw directly from the day's meetings, tickets, docs,
and comms. Do not invent or speculate; if a question has no answer from today's
data, write `_(nothing notable today)_`.

### lattice_update

Answer the five Lattice weekly update questions:
- **AI wins**: any AI tooling used, automation advanced, or AI-related project
  progress (e.g. segment registry pipeline, Claude integrations, MCP work).
- **Impact / highlights**: the most significant outcomes from today — decisions
  made, deliverables shipped, meetings with concrete outputs, docs published.
- **Next week focus**: synthesize from open action items and known project state
  to project what is coming; be specific (ticket IDs, project names).
- **Blockers**: any blockers or asks for help surfaced today.
- **Other**: anything else worth sharing with leadership.

### geekbot_em_update

Answer the four engineering-manager-updates questions:
- **Learned / discovered**: a specific insight or new information from today —
  could be a technical finding, a process gap, a team signal from a 1:1, or a
  cross-functional development.
- **Blocking / slowing**: any personal blockers or dependencies on others.
- **Next few days focus**: near-term priorities; be specific.
- **Flag for the group**: anything cross-functional, a risk, or something other
  EMs or leadership should know about.

### geekbot_tl_update

Answer the tech-leaders-updates questions:
- **Execution capacity**: honest team bandwidth assessment from signals today
  (blockers, manual workload, staffing, competing priorities).
- **Top outcomes / deliverables**: specific team outputs — reference ticket IDs,
  doc titles, pipeline milestones; draw from ticket_activity, doc_activity,
  and 1:1 meeting notes.
- **Blockers affecting delivery or partners**: anything that could slip delivery
  or create friction for partners or cross-functional teams.
- **Slowing execution**: impediments, resourcing gaps, process issues, or items
  that may require sprint adjustment or escalation.

## Step 6: render, write, commit

1. Render strictly per `schema/daily-log-schema.md` in the fixed section order:
   `tl;dr` → `meetings` → `meeting_notes` → `action_items` → `decisions` →
   `ticket_activity` → `doc_activity` → `comms_highlights` → `lattice_update` →
   `geekbot_em_update` → `geekbot_tl_update`.
   - `meetings`: one pipe-grammar line per meeting (index only).
   - `meeting_notes`: one block per meeting, ordered by start time. Each block
     has the `### Title`, `> time | source:` header, full AI summary verbatim,
     `**Discussion:**` bullets with timestamps, `**Decisions:**` list, and
     `**Actions:**` line. If a meeting has no Fellow file and no Zoom AI
     summary, write `_(no source content available)_`. Never skip a meeting
     that appears in the meetings index.
   - All other sections: one item per line, pipe grammar, `_(none)_` if empty.
2. Fill frontmatter: `fellow_source: repo`, counts, the three source-status
   lists, entities, capture_notes.
3. Write `daily/DATE.md`. Commit directly to `branch.name` and push. No PR, no
   claude/ branch. (previous mode is deterministic, so overwrite if it exists.)

## Step 7: report back

Return the manifest: DATE, the three source-status lists, counts, capture_notes.
