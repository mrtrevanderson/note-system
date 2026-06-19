# Fellow Dump Schema (v1)

The contract between the two routines. `fellow-dump` writes files in this format;
`daily-note` reads them. One file per meeting.

## Path

`fellow/{year}/{date}/{slug}.md`

- `{year}` is the four-digit year of the meeting day, local timezone (e.g. `2026`).
- `{date}` is the meeting day, `YYYY-MM-DD`, local timezone.
- `{slug}` is the title lowercased, spaces to hyphens, non-alphanumeric stripped.
  If two meetings share a slug, append the start time, e.g. `de-1-1-kapil-0900`.

## File contents

```
---
title: <meeting title>
date: <YYYY-MM-DD>
time: <HH:MM>-<HH:MM>
participants: [<name>, <name>]
fellow_url: <url>
captured_at: <ISO 8601 timestamp>
---

## AI Notes
<the full Fellow AI summary/notes, verbatim, not shortened>

## Transcript
<the full transcript, verbatim, not shortened>
```

## Rules

1. **Verbatim.** Both sections are copied whole. Never summarize or trim; that is
   the entire purpose of this layer.
2. **One file per meeting.** No combining, no index file.
3. **Frontmatter is stable.** daily-note parses `title`, `time`, and
   `participants` to match each meeting onto the Outlook calendar spine, so those
   keys must always be present even if a value is empty (`participants: []`).
4. **Empty day.** If there were no Fellow meetings, write nothing. daily-note
   treats a missing `fellow/{year}/{date}/` folder as "no Fellow meetings", not an error.
