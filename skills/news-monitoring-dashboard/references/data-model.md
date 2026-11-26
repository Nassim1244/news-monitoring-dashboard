# News Watch - Data model

Version 1.0.0 - 2026-11-26.

Store: the page's `db` capability (document store: even path segments = documents, odd = collections).
Writers: the page (the user's actions) and Claude (the run, via the database tools).

## 1. Principles

- **Two writers never touch the same document.** The run writes `items`, `tagStats`, `runs`, `profile`, `meta`, `seen`; the page writes `feedback`. `tags` and `sources` are shared but edited field by field (`merge`).
- **Feedback lives apart from items.** Items are purged after 3 months; feedback docs carry a snapshot of the item, so favourites and learning examples survive.
- **Lean Noise.** Noise items store no excerpt and no one-liner.
- **Stable ids.** `itemId` = first 16 hex chars of `sha1(canonicalUrl)`.
- **Period id.** Weekly cadence: ISO week `YYYY-Www` (e.g. `2026-W48`). Daily cadence: `YYYY-MM-DD`. The field is named `week` in both cases to keep one page template. Backfilled items take the period of their publication date.

## 2. Collections

### `items/{itemId}` - written by the run

| Field | Type | Notes |
|---|---|---|
| `url` | string | Original link |
| `canonicalUrl` | string | Lowercased host, no `www.`, no tracking params, no fragment, no trailing slash |
| `title` | string | In `displayLanguage` (translated if needed) |
| `titleOriginal` | string? | Original title, only when translated |
| `lang` | string | Language of the source article (`fr`, `en`) |
| `sourceId` | string | -> `sources/{sourceId}` |
| `publishedAt` | ISO date | From the feed |
| `week` | string | Period id |
| `runId` | string | -> `runs/{runId}` |
| `tags` | string[1..4] | Tag ids |
| `priority` | 1 / 2 / 3 | Best priority among `tags` |
| `bucket` | enum | `signal` / `watch` / `totry` / `brief` / `noise` (Claude's call) |
| `rank` | number | Order inside the bucket |
| `excerpt` | string? | ~300 chars, in `displayLanguage`. **Absent for Noise** |
| `oneLine` | string? | Claude's line, in `displayLanguage`. Only Signal, Watch, To try, Briefs |
| `corroboration` | number | Distinct sources mentioning the story this period |
| `mentions` | string[] | `sourceId`s that mentioned it |
| `storyKey` | string | `entity:event`, language independent, used for merge and persistence |
| `seenWeeks` | number | Periods the story has been active |
| `promotedFrom` | string? | `watch` when promoted by persistence |
| `firstWeek` | string? | First period stored (set on fold) |
| `updates` | array? | Fold history `[{week, line, sources[]}]` |

Effective bucket in the page = `feedback.bucketUser ?? items.bucket`.

### `feedback/{itemId}` - written by the page (seed examples by Claude)

| Field | Type | Notes |
|---|---|---|
| `rating` | 1 / 0.5 / 0 / null | 👍 / 🤷 / 👎 |
| `favourite` | bool | Never purged |
| `bucketClaude` | enum | Copied from the item at first action |
| `bucketUser` | enum? | Set when the user moves the card |
| `tryStatus` | enum? | `totry` / `tried` / `adopted` / `dropped` |
| `moves` | array | `[{from, to, at}]` |
| `hidden` | bool? | Card collapsed by the user. Ignored by the run |
| `snap` | object | `{title, url, sourceId, week, tags, oneLine, excerpt}` |
| `seed` | bool | `true` for seed examples from `MyTaste.md` (ids `seed-01`...). Hidden in the page and in tag ratios |
| `updatedAt` | ISO datetime | Used by the run to pick new feedback |

### `tags/{tagId}`

| Field | Type | Notes |
|---|---|---|
| `label` | string | In `displayLanguage` |
| `parent` | string? | Parent dot path |
| `priority` | 1 / 2 / 3 | |
| `keywords` | string[] | Include keywords in every source language |
| `status` | enum | `active` / `suggested` / `rejected` |
| `order` | number | Display order |
| `reason`, `examples`, `suggestedAt`, `decidedAt` | | Suggestions only |

### `tagStats/{week}` - written by the run

`{ week, tags: { "<tagId>": { n, signal } } }`. Rating ratios are computed live by the page.

### `sources/{sourceId}`

| Field | Type | Notes |
|---|---|---|
| `name`, `type` | string | |
| `lang` | string | `fr` / `en` |
| `access` | enum | `rss` / `atom` / `html` / `json` |
| `url` | string | |
| `role` | enum | `item` / `crosscheck` / `totry` |
| `enabled` | bool | Toggled from the Sources tab |
| `heavy` | bool | Read only entries newer than last run |
| `note` | string? | |
| `order` | number | |
| `lastFetch` | object | `{at, status: ok / empty / error, items, error?}` |
| `lastItemAt` | ISO date? | `stale` flag if older than 30 days |
| `failStreak` | number | `broken` flag at 3 |

### `profile/current` - written by the run

| Field | Type | Notes |
|---|---|---|
| `rules` | string | Rules from `MyTaste.md` (Signal, Watch, Briefs, Noise, To try, dislikes, interests, context) |
| `text` | string | Distilled taste profile, ~2,000 chars max |
| `version`, `feedbackCount`, `updatedAt` | | |

### `settings/current` - written at build time

`{ watchName, field, sourceLanguages: "fr" | "en" | "fr+en", displayLanguage, cadence: "weekly" | "daily", runDay, runTime, timeZone }`

### `runs/{runId}`

`runId` = `YYYY-MM-DDTHHmm` UTC. Fields: `startedAt, finishedAt, windowFrom, windowTo, backfill, weeks, counts {signal, watch, totry, brief, noise, fetched, folded, stored, translated}, failedSources, tagSuggestions, purged, cost {durationMin, tokensEst}, notes, log[]`.

### `seen/{week}`

`{ week, ids: [...] }`: hashes of entries not stored as items.

### `meta/state`

`{ schemaVersion: 1, lastRunId, lastRunAt, weeks: [...], sourcesSyncedAt, myTasteSyncedAt }`.

## 3. Access rules

Private page, single user. Every collection: `read: "view"`, `write: "owner"`.

## 4. Page queries

| Tab | Query |
|---|---|
| This week | `meta/state` (period list); `items where week == W`; `feedback` (whole collection) |
| To try | `feedback where tryStatus != null` + `items where bucket == totry` |
| Favourites | `feedback where favourite == true` (uses `snap`) |
| Tags | `tags` + last 12 `tagStats` + `feedback` |
| Sources | `sources` by `order` |

## 5. Volume

50 items per week, 13 weeks kept: about 650 item docs plus feedback. Check the database limits of your plan before raising the cap.
