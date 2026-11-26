---
generated: YYYY-MM-DD
source_of_truth: page database (sources collection) once the page exists
sync: before the page exists, this file is the source. After, the optional mirror add-on regenerates it; edits here are imported if the file is newer than the last sync, database wins on conflict.
---
# Sources

Columns: id | name | type | lang | access | url | role | on | notes
- lang: `fr` or `en`.
- access: `rss`, `atom`, `html`, `json`.
- role: `item` (shown), `crosscheck` (corroboration only, not shown), `totry` (To try candidates).
- on: `yes` / `no`.

| id | name | type | lang | access | url | role | on | notes |
|---|---|---|---|---|---|---|---|---|

# Excluded or deferred
| name | reason | option later |
|---|---|---|
