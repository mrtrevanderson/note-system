# Routine 1: fellow-dump

Runs first (~05:00 local). Copies yesterday's Fellow meetings verbatim into the
repo. Self-contained; it only needs the Fellow connector and the repo.

Paste this as the routine prompt:

```
Do not do the work in this top-level session. Immediately launch a subagent to
perform the entire task below, then report its result. (The subagent is what
makes the Fellow connector initialize in an unattended run.)

Subagent task: copy yesterday's Fellow meetings into this repository.

1. Read config.yaml and schema/fellow-dump-schema.md from the repo root.
2. Compute yesterday's date in identity.timezone. Call it DATE (YYYY-MM-DD).
3. Verify Fellow connector tools are available. If not, commit nothing and report
   "Fellow connector not initialized".
4. List all Fellow meetings that occurred on DATE. For each meeting, retrieve the
   full AI notes/summary, the full transcript, the participants, and the Fellow URL.
5. Write one file per meeting to fellow/DATE/<slug>.md, exactly per
   schema/fellow-dump-schema.md. Copy the AI notes and transcript VERBATIM; do
   not shorten anything.
6. If there were no meetings on DATE, commit nothing and report "no meetings".
7. Commit the new files directly to the branch in config branch.name and push.
   Do NOT open a pull request and do NOT use a claude/-prefixed branch.
8. Report back: DATE, number of meetings captured, and the file paths written.
```

## Setup checklist

- Repository: the note-system repo.
- Connectors: keep ONLY Fellow.ai; remove the rest.
- Permissions: enable **Allow unrestricted branch pushes** for the repo.
- Trigger: Scheduled, daily at the `schedule.fellow_dump_local` time (05:00).
- Validate with **Run now**, then confirm `fellow/<yesterday>/` appears on `main`.
```
