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
Zoom (from the connector) attach to it.
`- HH:MM-HH:MM | <title> | attendees: <name; name> | notes: <fellow_path|zoom_url|none> | outcome: <one factual clause or none> | actions: <action ids or none>`

For Fellow, `notes:` is the repo path, e.g. `fellow/2026-06-17/de-1-1-kapil.md`.

### `## action_items`
`- [ ] id:<AI-NNN> | owner:<name> | due:<YYYY-MM-DD|none> | source:<meeting title|jira DE-XXXX|slack|email> | <verbatim-as-possible text>`

### `## decisions`
`- <decision, factual> | context:<where> | entities:<tags>`

### `## ticket_activity`
One line per discrete Jira action.
`- <DE-XXXX> | <created|transitioned|commented|worklog> | <detail> | <url>`

### `## doc_activity`
`- <title> (<page_id>) | space:<KEY> | <created|edited> | <url>`

### `## comms_highlights`
Substantive Slack/email only (decision, ask, commitment, status change).
`- <slack|email> | <channel/thread or subject> | with:<name; name> | <the point, factual> | <url|none>`

## Empty-section convention

```
## decisions
_(none)_
```

See `daily/_TEMPLATE.md` for a complete example.
