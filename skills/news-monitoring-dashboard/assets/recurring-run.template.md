---
name: recurring-run
description: Scheduled news watch run. Fetch sources, classify all new entries (pass 1), consolidate (pass 2), translate to display language, write to page database, report.
kind: agent
page: {{PAGE_URL}}
page_id: {{PAGE_ID}}
db_tools: {{DB_TOOLS}}
---
## Talk rule
Prompt terse. Final report to user: normal short sentences, display language.

## Contract
### Preflight
- Requires: db tools load via ToolSearch (`db_tools` above)
  → Otherwise: STOP | recurring-run | db tools missing
- Requires: `settings/current`, `meta/state`, `profile/current` exist
  → Otherwise: STOP | recurring-run | database not seeded (run step 2)
- Requires: 1+ source `enabled: true` with `lang` in `sourceLanguages`
  → Otherwise: STOP | recurring-run | no enabled source
### Hand off
- Delivers: items for current period (cap: weekly 50, daily 12); `seen`, `tagStats`, `sources`, `profile/current`, `runs/{runId}`, `meta/state` updated
- Delivers: short report: items per bucket, per language, failed sources, tag suggestions, time, token estimate, page link
- Logs: every STOP and notable event in `runs/{runId}.log`, one line each
### Forbidden
- Never: long dash character
- Never: pass 1 candidates in database
- Never: write `feedback`; set a tag `active` or `rejected`
- Never: drop, demote to Noise or delete an item with a `feedback` doc
- Never: invent sources, tags, people, preferences
- Never: fetch a source whose `lang` not in `sourceLanguages`
- Never: shell fetch of news sites (use WebFetch)
### On failure
- When: source fails twice this run → `status: error`, next source
- When: db write version conflict twice on same call → STOP | recurring-run | db conflict on <path>
- When: session near half budget → finish current step, write done work with `notes: "partial"`, update `meta/state`, report

## Process
1. **Load.** `settings/current` (sourceLanguages, displayLanguage, cadence), `meta/state`, `profile/current`; query `sources`, `tags`. Few-shot: feedback `updatedAt > lastRunAt` + 20 latest with rating or `bucketUser` + `seed: true`.
2. **Window.** `runId` = UTC now `YYYY-MM-DDTHHmm`. Window = `lastRunAt` to now. Period = ISO week `YYYY-Www` (weekly) or `YYYY-MM-DD` (daily). `lastRunAt` null: window = now - 28 days, period = period of publish date (backfill).
3. **Fetch** enabled sources, WebFetch prompt:
   > List entries published after {windowFrom}, max {N}. One JSON line each: {"t": title, "u": link, "d": YYYY-MM-DD, "s": feed summary cut to 150 chars}. Digest: each distinct story as {"t", "u", "d", "m": 1}. Only JSON lines.

   N: official/news 25, blogs 10, newsletters 6, digests 40 stories, To try sources 10 (on-topic only). Add source `lang` to each line. Per source: `lastFetch {at, status ok|empty|error, items, error}`, `lastItemAt`, `failStreak`. All lines → workspace `pass1.jsonl`.
4. **Dedup.** Canonical URL: lowercase host, no `www.`, no `utm_*`/`ref`/`source` params, no fragment, no trailing slash. `itemId` = sha1(canonical)[:16] in Bash. Drop: older than window, id in `items` or `seen/{period}`.
5. **Pass 1 classify** → workspace `pass1.json`. Use `profile/current.rules`, `.text`, active tags (keywords all languages), few-shot.
   - Tags: 1-4 active ids; `priority` = best. `people.*` only if person subject or quoted.
   - `storyKey`: `entity:event`, English, lowercase, language independent (same story FR + EN = same key).
   - Bucket per rules. P3 = Noise unless big.
   - Digest story (`m: 1`): adds to matching entry `mentions`/`corroboration`; digest-only story = own entry.
6. **Pass 2 merge.** Same `storyKey` or same story → one candidate. `sourceId` = best (original over aggregator; tie → source in displayLanguage). All in `mentions`; `corroboration` = distinct sources.
7. **Pass 2 fold.** Query items bucket watch/signal, last 6 weeks → `{id, storyKey, title, bucket, week, version}`. Match → update that item: `week` = current, `firstWeek` = old if unset, append `{week, line, sources}` to `updates`, merge `mentions`, raise `corroboration`, `seenWeeks` + 1. Watch → Signal if active 2+ weeks or `corroboration` >= 3; `promotedFrom: "watch"`.
8. **Pass 2 score + quotas.** Score = priority + corroboration + taste match. Weekly targets: Signal 8 hard, Watch 12, To try 10, Briefs 5, Noise 15 (total 50). Daily: Signal 3, Watch 3, To try 2, Briefs 2, Noise 2 (total 12). Count folded + already stored for period. Over target: drop lowest. Spare → Watch, then Noise. Noise: half borderline (P1, 2+ sources), half spread across tags. Set `rank`.
9. **Translate.** Item `lang` != displayLanguage: `titleOriginal` = title; `title` = translation. Excerpt translated non-Noise only. Faithful, short, no added opinion.
10. **Write items.** Build ops in Bash (python) from `pass2.json`. 50 ops/call, one call at a time, 2 s pause. New: `set`. Folded: `update` + `ifVersion`. Fields: `url, canonicalUrl, title, titleOriginal?, lang, sourceId, publishedAt, week, runId, tags, priority, bucket, rank, corroboration, mentions, storyKey, seenWeeks`. Non-Noise only: `excerpt` (<= 300 chars), `oneLine` (1 sentence ~20 words, displayLanguage: why signal / watch / try; Briefs: what happened, why). If set: `promotedFrom`, `firstWeek`, `updates`.
11. **Write rest.** `seen/{period}` merge: ids not stored. `tagStats/{period}`: `n`, `signal` per tag. `sources/{id}` update + ifVersion: fetch fields. Suggestions: max 3, `tags/{id}` set `status: "suggested"`, label (displayLanguage), parent, `priority: 2`, keywords (all source languages), reason, examples (max 3 itemIds), suggestedAt; theme or person behind 3+ items over 2 periods; never a `rejected` id.
12. **Profile.** New feedback → rewrite `profile/current.text` (~2,000 chars max: likes, dislikes by tag and kind, bucket correction patterns); bump `version`; `feedbackCount`, `updatedAt`. Keep `rules`.
13. **Purge.** Cutoff 13 weeks. Delete older `items` unless feedback favourite; delete older `seen`, `tagStats`. Never delete `feedback`.
14. **Close.** `runs/{runId}` set: `startedAt, finishedAt, windowFrom, windowTo, backfill, weeks, counts {signal, watch, totry, brief, noise, fetched, folded, stored, translated}, failedSources, tagSuggestions, purged, cost {durationMin, tokensEst}, notes, log[]`. `meta/state` update + ifVersion: add period to `weeks`, drop purged, `lastRunId`, `lastRunAt` = run start.
15. **Report** to user (push notification / message if available, else final message). Include page link.
