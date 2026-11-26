# News Watch - Specification

Version 1.0.0 - 2026-11-26. Generic method. Your own values (field, languages, cadence, categories, sources) live in `config/`.

## 1. Goal

A personal news watch on one field that separates **signal** from **noise**.

- **Signal:** lasting shifts in the field (likely still relevant in 3 months or more). Not trends that vanish in days or weeks.
- **Watch:** promising but unconfirmed. Promoted to Signal if it persists.
- **To try:** concrete things the user can use now to improve their practice (tools, methods, templates, offers).
- **Briefs:** one-time events worth knowing (what happened, why), but not a trend.
- **Noise:** everything else, kept visible but collapsed.

The system learns two things from the user's feedback:
1. **Taste:** what they like (ratings, favourites).
2. **Signal judgment:** what they consider a lasting shift (manual bucket moves).

## 2. Scope

- Single user. No sharing, no multi-user logic.
- One field per watch. For a second field, build a second watch in a new workspace folder.
- Cadence set in `config/watch-settings.md`: weekly by default, daily as an option. Not real time.
- Languages set in `config/watch-settings.md` (section 3).

## 3. Languages

Two settings, chosen during the interview (step 0):

| Setting | Values | Effect |
|---|---|---|
| `sourceLanguages` | `fr`, `en`, or `fr+en` | Which sources are proposed and kept. Each source carries a `lang`. With `fr+en`, the run fetches both and merges the same story across languages (section 8) |
| `displayLanguage` | `fr` or `en` (any other language works the same way) | Language of the page UI, of Claude's one-liners, of tag labels, and of titles and excerpts shown |

Translation rules (token economy):
- An item in another language than `displayLanguage` gets its title translated, and its excerpt translated for non-Noise buckets only. The original title is kept in `titleOriginal`.
- Noise items: title translated only.
- The item keeps its `lang` so the page can show a small language badge (FR / EN).
- A source in a language outside `sourceLanguages` is never fetched.

## 4. Architecture

| Component | Role |
|---|---|
| Scheduled task (cloud) | `run/recurring-run.md`, at the day and time in `watch-settings.md`. Fetches sources via WebFetch, classifies, writes to the page database. Never needs the user's computer. |
| Page (published Artifact + `db` capability) | Shows the digest, collects ratings, moves, favourites, statuses. **Master store** for items, profile, tags, sources. |
| `config/watch-settings.md` | Field, languages, cadence, page id. Written by step 0, completed by step 2. |
| `config/MyTaste.md` | Readable taste profile and tag tree. Seeds the database. |
| `config/sources.md` | Readable source list. Seeds the database. |
| Scheduled task (computer, optional) | `run/mirror-local.md`: imports `sources.md` edits, regenerates `MyTaste.md` and `sources.md` from the database. |

Notes:
- Cloud sessions usually cannot reach news sites from the shell. All fetching goes through WebFetch.
- The cloud is master so the run never depends on a computer being on.

## 5. Buckets

| Bucket | Definition | Display |
|---|---|---|
| Signal | Persistent shift (likely relevant 3+ months), confirmed by several sources and/or several periods, matching the tag tree. Field-specific "Signal by default" rules live in `MyTaste.md` | Top 5 to 8, full card |
| Watch | Promising but unconfirmed. Promoted to Signal if it persists in later periods | Full card |
| To try | Actionable: tool, method, template, offer usable now | Full card + status, also in To try tab |
| Briefs | One-time event: leadership change at a major player, major M&A / IPO / shutdown, major incident, landmark ruling. Never promoted to Signal | Short list above Noise: title + one line "what happened, why" |
| Noise | The rest | Collapsed list, titles only, still rateable and movable |

## 6. Signal detection (persistence + judgment)

For each candidate story:
1. **Corroboration:** number of distinct sources mentioning it this period. Digests and newsletters are used as a cross-check sensor (section 9).
2. **Persistence:** whether the same story or topic appeared in previous periods (Watch history, tag trend).
3. **Judgment:** Claude's assessment against the tag tree, the taste profile and the user's past corrections (few-shot examples from recent moves and ratings).

Rule of thumb: Watch to Signal when the topic is still active 2+ weeks later or corroborated by 3+ independent sources. Tune after 3 or 4 weeks.

## 7. Learning

| Feedback | Meaning | Weight |
|---|---|---|
| 👍 / 🤷 / 👎 | Taste | 1 / 0.5 / 0 |
| Favourite | Taste | Strong 👍 |
| Move to bucket | Signal judgment | Stored as "Claude said X, the user said Y" |
| To try status | Taste + usefulness | Adopted > Tried > Dropped |

Each run, after classifying, Claude rewrites the distilled taste profile from new feedback and updates tag stats.

Why: a prompt-only description of preferences rates poorly; a written profile plus real past ratings as few-shot examples works much better, and costs far less than training a model.

## 8. Tag tree

Items are tagged, not placed in a single node: one article can belong to several branches.

- Each item carries **1 to 4 tags**. No primary tag.
- Tags are **hierarchical**, with stable dot-path ids (e.g. `business.sector.retail`).
- Each tag has a **priority** (1 high to 3 low), used as a cold-start weight: P1 = bonus, P2 = neutral, P3 = penalty (Noise unless big). An item takes the best priority among its tags. Ratings take over after a few weeks.
- Exceptions are allowed: a P3 branch can hold a P1 child (e.g. "infrastructure" P3, "major infrastructure projects" P1).
- Optional `people` branch: one tag per person of interest. An item gets the tag only when the person is the subject or is quoted.
- The starting tree comes from a pack in `references/packs/`, refined in step 1, stored in `MyTaste.md`.

Growth: each run may propose new tags (a theme or person behind 3+ items over 2 periods). They appear as **suggested** at the top of the Tags tab with Accept / Reject. Claude never adds a tag without approval, and never proposes a rejected one again.

Stats per tag: item count and signal count, 👍/🤷/👎 ratio, trend (sparkline), last activity.

## 9. Item content and token economy

- **Title + excerpt from the feed**, cut to about 300 characters. Claude writes nothing for these (except translation, section 3).
- HTML-only sources: title + teaser from the listing page.
- **One line by Claude** ("why signal / why watch / why try", or "what happened, why" for Briefs), only for Signal, Watch, To try and Briefs.
- **Digests and aggregator newsletters** (role `crosscheck`): not displayed. Used to count mentions (corroboration). A story found **only** in a digest becomes its own item.
- Dedup by canonical URL, then by title similarity. Across languages, the same story is merged by `storyKey` (entity + event), not by title.
- Seen index so entries are never processed twice.
- **Two passes per run.**
  - Pass 1 (collect and classify) reads every new entry, dedups, tags and buckets. Its output stays in the run workspace, never in the database.
  - Pass 2 (consolidate) produces what is stored:
    1. **Merge** same-story candidates into one item listing all sources. Prefer the original source over an aggregator, and a source in `displayLanguage` when both are equal.
    2. **Fold:** a candidate matching a Watch or Signal story stored in the last 6 weeks updates that item instead of creating one (moves to the current period, gains a dated update line and the new sources, may be promoted to Signal).
    3. **Score** = tag priority + corroboration + taste match.
    4. **Soft quotas, cap 50 items per week:** Signal 8 (hard), Watch 12, To try 10, Briefs 5, Noise 15. Over target: lowest scores drop. Spare room goes to Watch, then Noise. Daily cadence: cap 12 per day (Signal 3, Watch 3, To try 2, Briefs 2, Noise 2).
    5. **Noise sample:** half borderline (P1 tags, 2+ sources), half spread across tags.
    6. Everything not stored goes to the seen index.
- Items with feedback are never dropped by consolidation.
- Heavy feeds: read only entries newer than the last run.

Card fields: title, source, language badge, date, excerpt, tags, bucket, Claude's line (if any), link, actions (👍/🤷/👎, Favourite, Move to..., To try status when relevant, Hide) and Share (🔗 at the top right: opens the system share menu, or copies the link when there is none). Share is also in the card sheet.

**Hide.** Full cards only. Two-step button (Hide, then Confirm within 3 s). A hidden card collapses to one line (title link + Show) with status icons (⭐ if favourite, then the rating). Stored in `feedback.hidden`, synced across devices. Neutral for learning.

## 10. Page

**Mobile first.** Design for a ~380px viewport first, then desktop:
- Single column, no horizontal scroll, no hover-only actions.
- Tap targets at least 44px; actions reachable with the thumb.
- Bottom tab bar on mobile, top tabs on desktop.
- Noise and long lists collapsed; lazy rendering.
- Body text 16px minimum. Light and dark themes.
- UI text in `displayLanguage`, dates in its locale.

| Tab | Content |
|---|---|
| This week (or Today) | Period selector (3-month archive). Bucket count tiles act as filters. Signal, Watch, To try, Briefs sections, Noise collapsed. |
| To try | Backlog across periods with status: To try / Tried / Adopted / Dropped. |
| Favourites | All favourited items, kept beyond the archive. |
| Tags | Suggested tags (Accept / Reject) on top, tag tree with stats below. Tree collapsed by default, Expand all / Collapse all, search on label, id, keywords. |
| Sources | Source list with language, on/off toggle, last fetch status, stale and broken flags. |

## 11. Run pipeline

1. Load settings, sources, profile, tags and new feedback from the database.
2. Fetch each enabled source in `sourceLanguages` (WebFetch). Record fetch status.
3. Keep entries newer than the last run. Dedup.
4. Parse digests into story mentions (corroboration).
5. Tag each entry (1 to 4 tags); note candidate new tags.
6. Classify into the five buckets (sections 5 and 6, rules in `MyTaste.md`), then pass 2 (section 9).
7. Translate titles and excerpts not in `displayLanguage` (section 3). Write the one-liners in `displayLanguage`.
8. Update profile and tag stats; write everything to the database.
9. Purge non-favourite items older than 3 months.
10. Report to the user.

**First run:** backfill the last 4 weeks.

Data model: `references/data-model.md`.

## 12. Sources

Built in step 0 and stored in `config/sources.md`. Each source has: id, name, type, language, access (rss / atom / html / json), url, role, on/off.

| Role | Use |
|---|---|
| `item` | Its entries become items |
| `crosscheck` | Digest or aggregator: corroboration only, not shown |
| `totry` | Source of To try candidates (repositories, marketplaces, tool directories) |

Good mix for 15 to 25 sources: official sources of the main players, 3 to 5 specialist media, 2 to 4 expert blogs or newsletters, 2 to 4 digests as crosscheck, 1 or 2 To try sources, optionally a community feed with a popularity filter.

Every source is tested with WebFetch before it is added. Sources that block robots, need a login, or have a stale feed (last entry > 30 days) are listed as excluded with the reason.

## 13. Out of scope (v1)

- Real-time updates, notifications.
- Sources needing a login or blocking automated reading (social networks, video platforms).
- Multi-user, sharing.
- Trained ML model.
