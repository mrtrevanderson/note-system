# Routine 2: daily-note

Runs after fellow-dump (~06:00 local) so yesterday's transcripts are already
committed when this routine clones the repo.

Paste this as the routine prompt:

```
Do not perform the work in this top-level session. Immediately launch a subagent
to perform the entire task below, then report the subagent's manifest back.

Subagent task:

1. Read CLAUDE.md, config.yaml, daily-log-schema.md, and SKILL.md from the repo root.
2. Follow the instructions in SKILL.md (the daily-collector skill) exactly.
3. Capture yesterday (day_boundary.mode = previous) across every enabled source.
   Fellow comes from the repo at fellow/<YEAR>/<DATE>/ (already committed by the
   fellow-dump routine); do NOT call the Fellow connector. The other sources
   (Outlook, Zoom, Jira, Confluence, Slack) come from their live connectors.
4. Normalize into the daily-log schema, tag entities, and write daily/<DATE>.md.
   Commit it directly to the branch in config branch.name and push. Do NOT open a
   pull request and do NOT leave it on a claude/-prefixed branch.
5. Before exiting, verify connector tools are available. If none are, do not
   write an empty log: mark those sources failed with reason
   "connectors not initialized" and say so in the manifest.
6. Return the manifest: DATE, sources_captured / sources_failed / sources_skipped,
   counts, and any capture_notes.
```

## Setup checklist

- Repository: the same note-system repo.
- Connectors: attach Microsoft 365, Zoom, Atlassian, and Slack. Fellow is NOT
  needed here (it is read from the repo), but leaving it attached is harmless.
- Permissions: enable **Allow unrestricted branch pushes** for the repo.
- Trigger: Scheduled, daily at `schedule.daily_note_local` (06:00), a full hour
  after fellow-dump so transcripts are committed before this clones the repo.
- Validate with **Run now** AFTER a successful fellow-dump run for the same day,
  then open the run transcript (not just the status dot) to confirm it read
  fellow/<DATE>/ and committed daily/<DATE>.md to `main`.
```
