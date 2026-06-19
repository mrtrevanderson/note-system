# CLAUDE.md: note-system

This repo is driven by two unattended Cloud Routines that run with your laptop
closed. There is no human in the loop. Be conservative and deterministic.

## Two routines, one repo

1. **fellow-dump** (runs first, ~05:00 local). Copies yesterday's Fellow meetings
   verbatim into `fellow/{date}/`, one file per meeting. Raw capture, no analysis.
   Entry point: `routines/fellow-dump-prompt.md`. Output format:
   `schema/fellow-dump-schema.md`.

2. **daily-note** (runs later, ~06:00 local). Builds the structured daily log at
   `daily/{date}.md`. It reads yesterday's transcripts from `fellow/{date}/` in
   the cloned repo (it does NOT call the Fellow connector), and pulls the other
   sources live. Entry point: `routines/daily-note-prompt.md`. Logic:
   `.claude/skills/daily-collector/SKILL.md`. Output format:
   `schema/daily-log-schema.md`.

## The layered model

- **Bronze:** `fellow/` (raw transcripts) and `daily/` (structured daily log).
- **Silver / Gold (later):** a weekly agent, then status and monthly agents, all
  live in THIS repo, read the folders above, and write `weekly/`, `status/`, etc.
  Do not split them into other repos; same-repo reads are the point.

Prime directive for both routines: faithful, lossless capture. Do not summarize
for significance, rank, or editorialize. Synthesis happens in the silver/gold
layers, not here.

## Branch rule (read before changing anything)

Both routines commit to `branch.name` in `config.yaml`, which must be the repo's
default branch (`main`). daily-note clones the default branch to read `fellow/`,
so transcripts and logs must live there. Enable "Allow unrestricted branch pushes"
on both routines, and keep `main` free of branch-protection rules.

## Connector initialization

Cold cloud sessions can start without MCP connectors loaded. Both routine prompts
work around this by delegating to a subagent. If a run finds no connector tools,
that is the bug, not an empty day: record the affected sources as failed and say
so rather than committing a misleadingly empty file.

## Failure philosophy

A single source failing never aborts a run. Record failures in the manifest.
Capture Slack last (most likely to rate-limit). If `fellow/{date}/` is missing,
note it and continue; calendar and Zoom still cover meetings.
