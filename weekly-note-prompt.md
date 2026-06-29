# Routine 3: weekly-note

Runs every Monday morning (~07:00 local), after the daily-note for Monday has
run. Reads the previous week's daily logs and writes one weekly summary file.
No live connectors required — all input comes from the repo.

Paste this as the routine prompt:

```
Do not perform the work in this top-level session. Immediately launch a subagent
to perform the entire task below, then report the subagent's manifest back.

Subagent task:

1. Read CLAUDE.md, config.yaml, weekly-log-schema.md, and WEEKLY-SKILL.md from
   the repo root.
2. Follow the instructions in WEEKLY-SKILL.md (the weekly-collector skill) exactly.
3. Compute the previous Mon–Fri week in identity.timezone. Read all daily logs
   for that window from daily/YYYY-MM-DD.md. Note any missing dates.
4. Aggregate, deduplicate, and synthesize across all daily logs per the skill.
5. Write weekly/{year}/{week}.md (e.g. weekly/2026/2026-W26.md).
   Commit directly to the branch in config branch.name and push. Do NOT open a
   pull request and do NOT leave it on a claude/-prefixed branch.
6. Return the manifest: week label, daily logs read, daily logs missing, counts.
```

## Setup checklist

- Repository: the same note-system repo.
- Connectors: attach **Atlassian** for team Jira activity (Step 5 of the skill).
  All other connectors are optional — the routine reads daily logs from the repo
  and only calls Jira live for team member activity.
- Permissions: enable **Allow unrestricted branch pushes** for the repo.
- Trigger: Scheduled, weekly on Monday at `schedule.weekly_note_local` (07:00).
  This runs after the Monday daily-note (06:00) so Monday's log is already
  committed when the weekly routine clones.
- Validate with **Run now** after at least one full week of daily logs exist,
  then open the run transcript to confirm it read the daily files and committed
  weekly/{year}/{week}.md to main.
```
