# note-system

One repo, two unattended Claude Code Cloud Routines that run with your laptop
closed and commit your work history to Git. A later weekly/status/monthly layer
lives in this same repo and reads what these produce.

## How it works

```
05:00  fellow-dump   --> writes fellow/YYYY-MM-DD/<meeting>.md   (raw transcripts + AI notes)
06:00  daily-note    --> reads fellow/YYYY-MM-DD/ + live connectors
                         writes daily/YYYY-MM-DD.md               (structured daily log)
```

`fellow-dump` runs first and commits yesterday's Fellow meetings verbatim.
`daily-note` runs an hour later, and because routines clone the default branch at
the start of each run, it sees those transcripts already committed and reads them
from disk instead of calling the Fellow connector. The other five sources
(Outlook, Zoom, Jira, Confluence, Slack) it pulls live.

This producer/consumer split is deliberate: the transcripts are captured once,
early, and the note build can't be derailed by a Fellow API hiccup later.

## Layers

- **Bronze:** `fellow/` (raw) and `daily/` (structured daily log).
- **Silver/Gold (later):** weekly, status, monthly agents, all in THIS repo,
  reading the folders above and writing `weekly/`, `status/`, etc.

## Repo layout

```
config.yaml                              # shared config for both routines
CLAUDE.md                                # operating contract for both
routines/
  fellow-dump-prompt.md                  # routine 1: paste-in prompt + setup
  daily-note-prompt.md                   # routine 2: paste-in prompt + setup
schema/
  fellow-dump-schema.md                  # raw dump format (contract between routines)
  daily-log-schema.md                    # daily log format (contract for downstream)
.claude/skills/daily-collector/SKILL.md  # routine 2 logic
fellow/YYYY-MM-DD/<meeting>.md           # routine 1 output
daily/YYYY-MM-DD.md                      # routine 2 output
daily/_TEMPLATE.md                       # worked example
```

## Setup

1. Create the repo and push this scaffold:
   ```
   gh repo create note-system --private --source=. --push
   ```
   A fresh repo's `main` has no protection rules, which is what these routines
   need. Do not add branch protection to `main` on this repo.

2. Fill in `config.yaml`: confirm name/timezone, set the Slack
   `channel_allowlist`, extend the `roster`.

3. Create **two** routines at claude.ai/code/routines, both pointed at this repo,
   both with **Allow unrestricted branch pushes** enabled:
   - **fellow-dump**: prompt from `routines/fellow-dump-prompt.md`, Fellow.ai
     connector only, daily at 05:00.
   - **daily-note**: prompt from `routines/daily-note-prompt.md`, the Microsoft
     365 / Zoom / Atlassian / Slack connectors, daily at 06:00.

4. Validate in order: **Run now** on fellow-dump, confirm `fellow/<yesterday>/`
   lands on `main`, then **Run now** on daily-note and confirm it read those files
   and wrote `daily/<yesterday>.md`. Open the run transcripts to confirm, since a
   green status only means the session didn't crash, not that the task succeeded.

## The two rules that will break it if you skip them

1. **Same branch, the default branch.** Both routines commit to `main`. daily-note
   clones `main` to read `fellow/`, so if the transcripts were on another branch
   it would see nothing. Keep `branch.name: main` in config and on both routines.

2. **Order by time.** Routines have no dependency triggers. The 05:00 / 06:00
   stagger is what guarantees transcripts exist before the note build clones the
   repo. If you compress the gap and fellow-dump runs long, daily-note reads a
   stale tree.

## For downstream agents

Read `schema/daily-log-schema.md` as the contract. Glob `daily/*.md` by date.
Trust the frontmatter: `sources_failed` plus `capture_notes` tell you when a day
is partial. Raw transcripts are under `fellow/` if you need depth the daily log
doesn't carry. Entity tags are consistent across files, so rollups are a grep.
