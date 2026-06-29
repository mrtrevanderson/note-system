---
name: weekly-collector
description: Builds the weekly log. Reads the previous week's daily logs from
  daily/*.md, aggregates and deduplicates all sections, synthesizes final
  Lattice and Geekbot update drafts, and commits weekly/{year}/{week}.md.
  Invoked by the weekly-note Cloud Routine.
---

# weekly-collector

Invoked by `weekly-note-prompt.md` (inside a subagent). Read `config.yaml`,
`weekly-log-schema.md`, and `daily-log-schema.md` first.

## Step 1: compute the window

From `config.yaml`, `identity.timezone`: compute the previous full Mon–Fri week.
- WEEK_START = most recent Monday (the one that just ended), YYYY-MM-DD.
- WEEK_END   = that Friday, YYYY-MM-DD.
- WEEK_LABEL = ISO week string, e.g. `2026-W26`.
- YEAR       = four-digit year of WEEK_START.

The target daily logs are `daily/YYYY-MM-DD.md` for each date Mon–Fri in the
window. List them; note any missing as `daily_logs_missing` in the frontmatter.

## Step 2: read all daily logs

For each daily log in the window, parse every section:
- Frontmatter: date, sources_captured, sources_failed, counts, entities,
  capture_notes.
- `## meetings`: all meeting lines.
- `## meeting_notes`: all per-meeting blocks (carry forward verbatim as needed).
- `## action_items`: all action item lines.
- `## decisions`: all decision lines.
- `## ticket_activity`: all ticket lines.
- `## doc_activity`: all doc lines.
- `## comms_highlights`: all comms lines.
- `## lattice_update`: the day's draft answers to the five questions.
- `## geekbot_em_update`: the day's draft answers.
- `## geekbot_tl_update`: the day's draft answers.

Hold everything in memory keyed by date. This is the raw material for all
subsequent steps.

## Step 3: aggregate and deduplicate

### meetings
Collect all meeting lines from all daily logs. Order by date then start time.
Each meeting appears once. Carry the `notes:` fellow path and `actions:` ids.

### decisions
Collect all decision lines. Deduplicate by meaning: if the same decision
appears in multiple daily logs (e.g. re-stated in a follow-up meeting),
emit it once with all source dates listed. Add a `date:` field to each line.

### action_items
Collect all action item lines from the week. For each action item, check
whether a later daily log's `ticket_activity` or `comms_highlights` provides
evidence it was completed; if so, mark `[x]` and add `completed: YYYY-MM-DD`.
Add a `date:` field showing when the action was created.

### ticket_activity
Group all ticket lines by ticket ID. For each ticket, emit one line
summarizing all actions across the week (e.g. `commented 2026-06-22;
transitioned In Progress->Review 2026-06-24`) with a summary of what happened.

### doc_activity
Group all doc lines by page_id. Emit one line per page with a `dates:` field
listing every day it was touched. Carry the detail description from the most
recent daily log entry for that page.

### comms_highlights
Collect all comms lines. Deduplicate threads that appear across multiple days
(same URL or same DM partner + same topic). Add a `date:` field. Keep the
most substantive version of the detail text.

## Step 4: synthesize update drafts

Using the full week's raw material — all meetings, action items, decisions,
tickets, docs, comms, and the five daily draft sections — write final weekly
drafts for the three standing update sections. These should reflect the
complete Mon–Fri picture, not just the most recent day.

### lattice_update (weekly synthesis)
- **AI wins**: aggregate all AI-related work across the week; be specific
  (tools used, automations advanced, pipelines progressed).
- **Impact / highlights**: the 3–5 most significant outcomes of the week —
  decisions made, deliverables shipped, docs published, projects unblocked.
  Cite dates and ticket/page IDs where relevant.
- **Next week focus**: synthesize from open action items and known project
  state; project the top 3–4 priorities entering next week.
- **Blockers**: any blockers that remain unresolved entering next week.
- **Other**: cross-functional items, leadership signals, or anything else
  worth sharing.

### geekbot_em_update (weekly synthesis)
- **Learned / discovered**: the most significant insight or new information
  from the week — a technical finding, a team signal, a process gap.
- **Blocking / slowing**: any personal blockers or unresolved dependencies.
- **Next few days focus**: top personal priorities for the coming week.
- **Flag for the group**: cross-functional risks, team health signals, or
  items other EMs or leadership should be aware of.

### geekbot_tl_update (weekly synthesis)
- **Execution capacity**: honest team bandwidth assessment for the week —
  manual workload burden, blocker density, staffing notes.
- **Top outcomes / deliverables**: the team's most significant outputs for
  the week; reference ticket IDs, doc titles, pipeline milestones.
- **Blockers affecting delivery or partners**: anything that slipped or
  could affect partners or cross-functional teams entering next week.
- **Slowing execution**: impediments, process friction, resourcing gaps,
  or items that may require sprint adjustment or escalation.

## Step 5: compute frontmatter counts and entities

- `counts.meetings`: total meeting lines.
- `counts.decisions`: total deduplicated decisions.
- `counts.action_items_open`: action items with `[ ]`.
- `counts.action_items_closed`: action items with `[x]`.
- `counts.tickets_touched`: unique ticket IDs across the week.
- `counts.docs_touched`: unique page IDs across the week.
- `entities`: union of all entity sets from the daily logs, deduped and sorted.
- `daily_logs_read`: dates of successfully read daily logs.
- `daily_logs_missing`: dates in the Mon–Fri window with no daily log found.

## Step 6: read historical weekly logs for trend sections

Read the following files from `weekly/{year}/` (and prior years if the quarter
spans a year boundary). Parse only the YAML frontmatter of each file — do NOT
re-read the full body.

### weekly_trend
Find up to 3 weekly log files with `week_start` < WEEK_START, sorted
descending by date, take the 3 most recent. Extract from each:
`week`, `counts.meetings`, `counts.decisions`, `counts.action_items_open`,
`counts.action_items_closed`, `counts.tickets_touched`.
Hold alongside the current week's counts. The table has 2–4 rows total.

### q3_progress
Read `quarter.start` and `quarter.end` from `config.yaml`.

- If WEEK_START < `quarter.start`: set `q3_not_started = true`.
- Otherwise: read ALL weekly logs with `week_start >= quarter.start` AND
  `week_start <= WEEK_START`. Sort chronologically. Extract `week`,
  `counts.action_items_closed`, `counts.tickets_touched`,
  `counts.meetings` from each.
  - Compute cumulative `∑ AI Closed` and `∑ Tickets` row by row.
  - `WEEK_NUM` = count of rows (i.e. how many Q3 weeks so far).
  - `TOTAL_WEEKS` = round((quarter.end − quarter.start).days / 7) = 13
    for a standard quarter.
  - `MAX_Y` for the Mermaid y-axis = the larger of:
      - max weekly tickets_touched × 2
      - cumulative tickets so far × (TOTAL_WEEKS / WEEK_NUM) × 1.2
    rounded up to the nearest 50, minimum 100.

## Step 7: render, write, commit

1. Render per `weekly-log-schema.md` in the fixed section order:
   `tl;dr` → `meetings` → `decisions` → `action_items` → `ticket_activity` →
   `doc_activity` → `comms_highlights` → `lattice_update` → `geekbot_em_update`
   → `geekbot_tl_update` → `weekly_trend` → `q3_progress`.

   **`weekly_trend` format** — bold the current week row:
   ```
   ## weekly_trend
   > Rolling 4-week snapshot

   | Week    | Meetings | Decisions | AI Open | AI Closed | Tickets |
   |---------|----------|-----------|---------|-----------|---------|
   | W23     | 18       | 32        | 45      | 5         | 41      |
   | **W26** | **23**   | **47**    | **69**  | **6**     | **57**  |
   ```

   **`q3_progress` format** — bold the current week row; include the chart:
   ```
   ## q3_progress
   > Q3 2026 — Week 4 of 13

   | Week    | AI Closed | Tickets | Meetings | ∑ AI Closed | ∑ Tickets |
   |---------|-----------|---------|----------|-------------|-----------|
   | W28     | 8         | 45      | 18       | 8           | 45        |
   | **W31** | **9**     | **61**  | **22**   | **34**      | **206**   |

   ```mermaid
   xychart-beta
       title "Q3 2026 — Tickets touched"
       x-axis ["W28", "W29", "W30", "W31"]
       y-axis "Count" 0 --> 300
       bar [45, 52, 48, 61]
       line [45, 97, 145, 206]
   ```
   ```

   If `q3_not_started`:
   ```
   ## q3_progress
   > Q3 2026 starts 2026-07-01

   _(not yet started)_
   ```

2. Write `weekly/YEAR/WEEK_LABEL.md` (e.g. `weekly/2026/2026-W26.md`).
   Create the year subfolder if it does not exist.
3. Commit directly to `branch.name` and push. No PR, no claude/ branch.
   Overwrite if the file already exists.

## Step 8: report back

Return the manifest: WEEK_LABEL, daily_logs_read, daily_logs_missing, counts,
and any notes about deduplication or synthesis decisions made.
