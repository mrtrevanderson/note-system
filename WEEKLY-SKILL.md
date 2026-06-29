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

### pr_activity
Aggregate all `pr_activity` lines from the daily logs. Deduplicate by PR
number — if the same PR appears across multiple days, merge into one line
showing its full arc (opened Mon, merged Thu). Supplement with live data:
call `getTeamworkGraphContext` on all tickets in `ticket_activity` with
`targetObjectTypes: [ExternalPullRequest]` to surface any linked PRs not
already captured in the daily logs. If neither daily log data nor Atlassian
graph data is available, write `_(PR data unavailable this week)_`.

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

## Step 5: pull team activity from Jira

Query Jira for all DE team members in `roster` over the full week window
(WEEK_START 00:00 to WEEK_END 23:59, identity.timezone). This is a live
connector call; wrap in an error boundary and note any failure in the manifest.

For each roster member:
1. Look up their Jira account ID via `lookupJiraAccountId` (use their email
   from config if available, otherwise their display name).
2. Query `searchJiraIssuesUsingJql`:
   ```
   (assignee = "<accountId>" OR reporter = "<accountId>")
   AND project in (DE)
   AND updated >= "WEEK_START" AND updated <= "WEEK_END"
   ```
3. For each issue returned, call `getJiraIssue` and filter to activity
   (comments, transitions, worklogs) authored by that person in the window.
4. Record: person name, list of ticket IDs with a one-line description of
   what they did on each (created, commented, transitioned From→To, worklog Xh).

Aggregate into the `## team_activity` section:
- One `### Name` block per person with activity, ordered by ticket count desc.
- Members with zero activity: a single trailing line listing their names.
- Team summary line with total tickets and unique team members active.
- Hold per-person ticket counts for the Chart 4 Mermaid bar.

If the Jira connector is unavailable, write `_(team Jira data unavailable this
week)_` in the section and skip Chart 4.

## Step 6: generate charts

From the per-day counts (Step 2) and team activity data (Step 5):

Generate four Mermaid `xychart-beta` blocks. Use the short day label
`Mon MM-DD` for X-axis labels on the daily charts.

**Chart 1 — Meeting load**: one bar per day, Y = meeting count from that
day's frontmatter `counts.meetings`. Set the Y-axis upper bound to
`max(counts) + 2` rounded up to the nearest 2.

**Chart 2 — Action item velocity**: two bars total for the week:
- "Created" = sum of all `counts.action_items` across the daily logs.
- "Closed" = count of `[x]` action items identified in Step 3.
Set Y-axis upper bound to `max(created, closed) + 3`.

**Chart 3 — Daily activity breakdown**: three lines across Mon–Fri:
- Line 1 (tickets): `counts.ticket_activity` per day.
- Line 2 (docs): `counts.doc_activity` per day.
- Line 3 (comms): `counts.comms_highlights` per day.
Set Y-axis upper bound to `max(all values) + 3` rounded up.

If a day's log is missing, use `0` for all its values and note the gap in
the chart title (e.g. `"Meetings per day (Wed missing)"`).

**Chart 4 — Team ticket activity**: X-axis = first names of team members with
activity this week, ordered descending by ticket count. Y-axis = tickets touched.
Omit members with zero activity. Skip this chart entirely if Step 5 failed.

## Step 6: compute frontmatter counts and entities

- `counts.meetings`: total meeting lines.
- `counts.decisions`: total deduplicated decisions.
- `counts.action_items_open`: action items with `[ ]`.
- `counts.action_items_closed`: action items with `[x]`.
- `counts.tickets_touched`: unique ticket IDs across the week.
- `counts.docs_touched`: unique page IDs across the week.
- `entities`: union of all entity sets from the daily logs, deduped and sorted.
- `daily_logs_read`: dates of successfully read daily logs.
- `daily_logs_missing`: dates in the Mon–Fri window with no daily log found.

## Step 7: render, write, commit

1. Render per `weekly-log-schema.md` in the fixed section order:
   `tl;dr` → `team_activity` → `charts` → `meetings` → `decisions` →
   `action_items` → `ticket_activity` → `doc_activity` → `pr_activity` →
   `comms_highlights` → `lattice_update` → `geekbot_em_update` →
   `geekbot_tl_update`.
2. Write `weekly/YEAR/WEEK_LABEL.md` (e.g. `weekly/2026/2026-W26.md`).
   Create the year subfolder if it does not exist.
3. Commit directly to `branch.name` and push. No PR, no claude/ branch.
   Overwrite if the file already exists.

## Step 8: report back

Return the manifest: WEEK_LABEL, daily_logs_read, daily_logs_missing, counts,
and any notes about deduplication or synthesis decisions made.
