# Step 2 - Build page, seed database, first run, schedule

## Talk rule
Prompt terse. Talk to user: normal full sentences, user's language. Pause for user OK after each phase.

## Read first, fully
- `references/spec.md`, `references/data-model.md`.
- `config/watch-settings.md`, `config/MyTaste.md`, `config/sources.md`. One missing or empty = STOP, tell user which step to run (0 or 1).
- `assets/index.template.html`.
- `assets/recurring-run.template.md`.

## Tools
- Artifact tool (publish page, `db` capability). Load `artifact-capabilities` and `artifact-design` skills if listed.
- Database tool: find via ToolSearch (`ArtifactData`, or `db_get db_query db_write`). Note exact names: step 3 needs them.

## Phase 1 - Page
1. Copy `assets/index.template.html` to `artifact/index.html`. Replace:
   - `{{WATCH_NAME}}` = watchName.
   - `{{UI_LANG}}` = displayLanguage (`fr`, `en`).
   - `{{LOCALE}}` = `fr-FR` or `en-GB` (or matching locale).
2. displayLanguage not `en`: translate every UI string in the script (tabs, buckets, buttons, empty states, toasts, aria labels). Keep data values (`signal`, `watch`, `totry`, `brief`, `noise`, try statuses) unchanged.
3. Add on card: small language badge from `item.lang` (FR / EN) when sourceLanguages = `fr+en`. If `titleOriginal`: show it as link `title` attribute or small second line.
4. cadence `daily`: tab "This week" becomes "Today" (translated), period label = date, not ISO week.
5. Keep: mobile first, light + dark, 16px body, 44px taps, no long dash.
6. Publish. Declare `db` capability, every collection `read: "view"`, `write: "owner"`. Private.
7. Write pageUrl + pageId into `config/watch-settings.md`. Send link. Wait OK.

## Phase 2 - Seed database
Batch writes (max 50 ops/call). Docs per `references/data-model.md`:
- `settings/current` from watch-settings.
- `tags/{id}` from MyTaste tree: label, parent, priority, keywords, `status: active`, order.
- `sources/{id}` from sources.md: name, type, lang, access, url, role, `enabled` (on), heavy (note says heavy), note, order, `failStreak: 0`.
- `profile/current`: `rules` = MyTaste sections Signal, Watch, Briefs, Noise, To try, Dislikes, Interests, Context (verbatim). `text` = short distilled profile from same. `version: 1`.
- `feedback/seed-NN` per seed example: rating (👍 1, 🤷 0.5, 👎 0), `bucketClaude` = `bucketUser` = bucket, `seed: true`, `snap {title, url, tags}`, updatedAt.
- `meta/state`: `schemaVersion: 1, lastRunId: null, lastRunAt: null, weeks: []`.
Check page shows tags + sources. Wait OK.

## Phase 3 - Recurring run prompt + first run
1. Copy `assets/recurring-run.template.md` to `run/recurring-run.md`. In the copy replace `{{PAGE_ID}}`, `{{PAGE_URL}}`, `{{DB_TOOLS}}` (exact tool names found above). Nothing else.
2. Execute `run/recurring-run.md` now as **first run, 4-week backfill**.
3. Report: items per bucket, per language, translated count, failed sources, time, token estimate. Wait OK.

## Phase 4 - Schedule
- Create scheduled task: cadence, day, time, time zone from watch-settings. Task prompt = content of `run/recurring-run.md`.
- Tell user: schedule, approval setting, how to pause it.
- Optional: offer the local mirror if user has the Claude desktop app and a local folder: copy `assets/mirror-local.template.md` to `run/mirror-local.md`, fill its placeholders, schedule it.

## Rules
- No long dash character.
- Never invent sources, tags, preferences. Use files.
- Low tokens: excerpts from feeds; Claude lines only Signal, Watch, To try, Briefs.
- List any spec point you had to decide, and how.
