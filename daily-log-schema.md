# Daily Agent Log Schema (v1)

The contract for `daily/{date}.md`. Written by daily-note; read by the weekly,
status, and monthly agents.

## Design rules

1. **Lossless over pretty.** The consumer is a machine. Capture faithfully; do
   not summarize, rank, or editorialize.
2. **Stable shape.** Every section appears in every file, in this order, even
   when empty (`_(none)_` marks an intentionally empty section so a parser can
   tell it from a missing one).
3. **One item per line**, using the pipe grammar per section. No nested bullets.
4. **Source failure is data.** A failed connector still renders what was
   captured; the failure goes in `sources_failed` with a reason in
   `capture_notes`. Note that Fellow comes from the repo, not a connector, so a
   Fellow problem shows as a capture_note about missing `fellow/{date}/` files.
5. **Entities tagged consistently** so cross-day rollups are a grep.

## Frontmatter

```yaml
---
schema_version: 1
type: daily-agent-log
date: 2026-06-17
timezone: America/Los_Angeles
generated_at: 2026-06-18T06:03:00-07:00
boundary_mode: previous
fellow_source: repo                   # transcripts read from fellow/{date}/
sources_captured: [outlook, fellow, zoom, jira, confluence]
sources_failed:   [slack]
sources_skipped:  []
counts: {meetings: 4, action_items: 6, decisions: 2, ticket_activity: 9, doc_activity: 3, comms_highlights: 5}
entities:
  tickets:  [DE-1351, DE-1525]
  people:   ["Kapil Sreedharan", "Jean Carlo Camacho"]
  pages:    [4677435393]
  keywords: ["Unity Catalog", "Q3 Planning"]
capture_notes:
  - "slack: rate limited after 2 channels; partial capture"
---
```

## Body sections (fixed order)

### `## tl;dr`
One flat factual orientation line. Not a significance summary.

### `## meetings`
The Outlook calendar event is the canonical object; Fellow (from the repo) and
Zoom (from the connector) attach to it. One index line per meeting.
`- HH:MM-HH:MM | <title> | attendees: <name; name> | notes: <fellow_path|zoom_url|none> | outcome: <one factual clause or none> | actions: <action ids or none>`

For Fellow, `notes:` is the repo path, e.g. `fellow/2026-06-17/de-1-1-kapil.md`.

### `## meeting_notes`
Full per-meeting detail for every meeting that has a Fellow file or Zoom AI
summary. One block per meeting, ordered by start time. Meetings with no source
content get a block stating that explicitly — never omit the block entirely.

```
### <meeting title>
> <HH:MM-HH:MM> | source: <fellow_path|zoom_url|none>

<AI summary: full text, verbatim from Fellow AI Summary / Zoom AI summary.
 If multiple paragraphs, keep them all.>

**Discussion:**
- [HH:MM:SS] <point — one bullet per chapter item or significant transcript moment. Preserve timestamps. Keep full detail; do not compress.>

**Decisions:**
- <decision text, verbatim>

**Actions:** <AI-NNN (owner: name); ...| none>
```

Rules:
- Copy the AI summary verbatim; do not paraphrase or shorten.
- For each chapter in the Fellow AI summary, emit all bullet points with their
  timestamps. If there are no chapters, extract key moments from the transcript.
- If the meeting has no AI summary and no transcript, write
  `_(no Fellow AI summary or transcript available)_` in place of the summary.
- Decisions and actions listed here must match those in `## decisions` and
  `## action_items` exactly.

### `## action_items`
`- [ ] id:<AI-NNN> | owner:<name> | due:<YYYY-MM-DD|none> | source:<meeting title|jira DE-XXXX|slack|email> | <verbatim-as-possible text>`

### `## decisions`
`- <decision, factual> | context:<where> | entities:<tags>`

### `## ticket_activity`
One line per discrete Jira action.
`- <DE-XXXX> | <created|transitioned|commented|worklog> | <detail> | <url>`

### `## doc_activity`
One line per page. The detail field describes what the page covers and, where
discernible from the content, what was added or changed in this session.

`- <title> (<page_id>) | space:<KEY> | <created|edited> | <detail: what the page covers and what was worked on — section names, key content, decisions documented, tables updated, etc.> | <url>`

### `## pr_activity`
Pull request and commit activity from GitHub for the day. Covers all repos
in the DE org/team. One line per PR event or notable commit.

`- <repo> | PR #<N> <title> | <opened|merged|closed|reviewed|commented> | author:<name> | <detail: what the PR does, what the review said, CI status> | <url>`
`- <repo> | commit <short-sha> | author:<name> | <commit message> | <url>`

Only capture substantive events: PRs opened/merged/closed, review approvals
or rejections with comments, CI failures. Skip automated bot commits and
passing CI noise unless they represent a meaningful signal.

### `## comms_highlights`
Substantive Slack/email. Cast wide: DM threads with any work content, channel
messages where the identity participated substantively, emails with decisions or
asks. One line per thread/email. The detail field should capture the full
substance of the exchange — what was asked, what was answered, what was decided
or left open — not just the headline.

`- <slack-dm|slack-channel|slack-mention|email> | <channel/DM partner/subject> | with:<name; name> | <full substance: question asked, answer given, decision made, blocker raised, next step agreed — be specific and complete> | <url|none>`

## Section order (fixed)

1. `## tl;dr`
2. `## meetings`
3. `## meeting_notes`
4. `## action_items`
5. `## decisions`
6. `## ticket_activity`
7. `## doc_activity`
8. `## pr_activity`
9. `## comms_highlights`
10. `## lattice_update`
11. `## geekbot_em_update`
12. `## geekbot_tl_update`

### `## lattice_update`
Draft answers to the five Lattice weekly update questions, based on the day's
captured activity. Label clearly as a draft. Answer each question with specific,
factual content drawn from meetings, tickets, docs, and comms. "Next week focus"
and "blockers" should reflect open action items and known blockers from the day.

```
## lattice_update
> draft — review before posting

**AI wins:**
<specific AI-related work, tools used, automations built or advanced>

**Impact / highlights:**
<the most significant outcomes, deliverables, or progress from today>

**Next week focus:**
<what is planned or expected next based on open action items and project state>

**Blockers:**
<any blockers or asks for support identified today; _(none)_ if clear>

**Other:**
<anything else worth sharing with Dan and leadership>
```

### `## geekbot_em_update`
Draft answers to the four engineering-manager-updates Geekbot questions.

```
## geekbot_em_update
> draft — review before posting to #engineering-manager-updates

**What's one thing you learned or discovered since last check-in?**
<specific insight, finding, or new information from today's meetings or work>

**Is anything blocking you or slowing you down?**
<blockers from today; _(none)_ if clear>

**What's your focus for the next few days?**
<near-term priorities based on open action items and project state>

**Anything you want to flag for the group?**
<cross-functional issues, risks, or items worth leadership awareness>
```

### `## geekbot_tl_update`
Draft answers to the tech-leaders-updates Geekbot questions.

```
## geekbot_tl_update
> draft — review before posting to #tech-leaders-updates

**Team execution capacity this week:**
<honest assessment of the team's bandwidth and throughput based on today's signals>

**Team's top outcomes or deliverables this week:**
<specific completed or advanced deliverables; reference tickets, docs, meetings>

**Blockers affecting delivery, partner experience, or cross-functional alignment:**
<blockers that may affect sprint delivery, partners, or other teams; _(none)_ if clear>

**Anything slowing team execution:**
<impediments, resourcing gaps, process friction, or escalation needs>
```

## Empty-section convention

```
## decisions
_(none)_
```

`## meeting_notes` uses `_(none)_` only when there were literally zero meetings.

See `daily/_TEMPLATE.md` for a complete example.
