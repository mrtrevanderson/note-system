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

- List files under `fellow/DATE/`. Each is one meeting in the fellow-dump schema:
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
- Record the file path (e.g. `fellow/DATE/de-1-1-kapil.md`) as the meeting's
  `notes:` value in the meetings index line.
- If `fellow/DATE/` is missing or empty: add `fellow: no transcripts in repo
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
- Capture title, page id, space, created|edited, url. Do not pull page bodies.

### Slack (last, most fragile)

- Always: DMs in the window and messages mentioning the identity
  (`slack_search_public_and_private`).
- Allowlisted channels (`slack.channel_allowlist`): `slack_read_channel` over the
  window for the identity's substantive messages and threads participated in;
  `slack_read_thread` for parent context. Never read `slack.channel_blocklist`.
- Apply the comms filter hard. On rate limit, keep what you have, note
  `slack: rate limited, partial capture`, and only mark failed if you got nothing.

## Step 3: the comms filter (Slack + email)

Keep an item only if it is a decision/direction, an ask, a commitment, or a
status change on real work. Drop greetings, acknowledgements, logistics, chatter,
and automated mail. When a kept item also carries an action or decision, also
lift it into the proper section. Err slightly toward keeping; this is bronze.

## Step 4: normalize and tag entities

Over every captured text block: tickets via `entities.patterns.ticket`, page ids
via `entities.patterns.confluence_page_id`, people via case-insensitive `roster`
match (emit canonical name), keywords via verbatim `entities.keywords` match.
Fill per-line `entities:` fields and build the frontmatter `entities` union
(deduped, sorted).

## Step 5: render, write, commit

1. Render strictly per `schema/daily-log-schema.md` in the fixed section order:
   `tl;dr` → `meetings` → `meeting_notes` → `action_items` → `decisions` →
   `ticket_activity` → `doc_activity` → `comms_highlights`.
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

## Step 6: report back

Return the manifest: DATE, the three source-status lists, counts, capture_notes.
