---
name: mirror-local
description: Optional. Runs on user computer after cloud run. Imports sources.md edits into page database, regenerates config/sources.md and config/MyTaste.md from database.
kind: agent
page_id: {{PAGE_ID}}
db_tools: {{DB_TOOLS}}
local_folder: {{LOCAL_FOLDER}}
---
## Setup (once)
Replace `{{PAGE_ID}}`, `{{DB_TOOLS}}` (from `config/watch-settings.md` and `run/recurring-run.md` header), `{{LOCAL_FOLDER}}` = workspace folder path on computer. Schedule on computer (Claude desktop app) 1 to 2 h after cloud run.

## Talk rule
Prompt terse. Report to user: one normal sentence, display language.

## Contract
### Preflight
- Requires: db tools load via ToolSearch → Otherwise: STOP | mirror-local | db tools missing
- Requires: `local_folder` readable → Otherwise: STOP | mirror-local | folder unreachable
### Hand off
- Delivers: `config/sources.md`, `config/MyTaste.md` match database; `meta/state.sourcesSyncedAt`, `myTasteSyncedAt` updated
- Delivers: one-line report: files changed, sources imported
### Forbidden
- Never: long dash character
- Never: write `items`, `feedback`, `tags`, `profile`, `runs`
- Never: retype a file from tool output; edit via python read-modify-write
### On failure
- When: db write version conflict twice → STOP | mirror-local | db conflict on <path>

## Process
1. **Load.** `meta/state`, `profile/current`, `settings/current`; query `sources` (by `order`), `tags` (`status: active`, by `order`).
2. **Import sources.md.** File mtime > `meta.sourcesSyncedAt`: parse table; apply `on`, `role`, `notes` changes and new rows (new id) to `sources` (update/set + ifVersion). Database wins if source `updatedAt` > file mtime.
3. **Regenerate `config/sources.md`.** Same layout as `assets/sources.template.md`. Excluded table unchanged.
4. **Regenerate `config/MyTaste.md`.** Front matter version + 1, updated = today. `# Tag model` unchanged. `# Topic tree` from active tags (`- id: <id> | label: <label> | priority: <p>` + `| keywords: ...`, 2-space indent per level). Rules sections = `profile/current.rules`. Add `# Distilled profile` = `profile/current.text`. `# Seed examples` unchanged.
5. **Git** (only if folder is a git repo): commit `chore(config): mirror run <meta.lastRunId>`.
6. **Sync dates.** `meta/state` update + ifVersion: `sourcesSyncedAt`, `myTasteSyncedAt` = now.
7. **Report.**
