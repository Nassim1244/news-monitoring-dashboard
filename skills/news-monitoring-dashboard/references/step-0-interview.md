# Step 0 - Interview: field, languages, cadence, sources

## Talk rule
Prompt terse. Talk to user: normal full sentences, warm, user's language (detect from first message). Ask in rounds with AskUserQuestion if available, else short numbered questions. Max 4 questions per round. Give recommended option each time.

## Read first
- `references/spec.md` (sections 2, 3, 12).
- `references/packs/` (list + headers only).
- `assets/watch-settings.template.md`, `assets/sources.template.md`.
- `references/examples/ai-watch/sources.md` = format example.

## Goal
Fill `config/watch-settings.md` + `config/sources.md`. Sources tested.

## Rounds
1. **Field.** What field? Why follow it (job, clients, decisions)? Who is user (role, sector, country)? Name for the watch (propose 3).
2. **Languages.** Two separate questions, both mandatory:
   - Source languages: French only / English only / both.
   - Display language (page, tag labels, Claude lines, translated titles): French / English / other.
   Explain once: other-language titles + excerpts get translated, original title kept.
3. **Cadence.** Weekly (recommend) or daily. Day, time, time zone. Weekly default: Friday 07:00 user time zone.
4. **Packs.** Show pack list (`references/packs/*.md` title + one line). User picks 0 to 3. No fit = blank tree built in step 1.
5. **Sources.**
   - Ask user own sources first (sites, newsletters, blogs, experts they trust).
   - Add pack candidate sources matching source languages.
   - Search web for more: official players, specialist media, expert blogs/newsletters, digests (role crosscheck), To try sources. Only languages chosen.
   - Target 15 to 25. Mix per spec 12.
   - **Test each** with WebFetch: feed URL works? last entry < 30 days? Find RSS/Atom first (`/feed`, `/rss`, `/atom.xml`, page `<link rel="alternate">`), else html listing.
   - Fail (robots, login, blocked, stale) = Excluded table + reason.
   - Show table to user: keep / drop / role. Wait OK.

## Write
- `config/watch-settings.md` from template. pageUrl, pageId empty.
- `config/sources.md` from template. id = short slug. lang column filled.
- Summary to user: settings + source count per role + per language + excluded. Next step: run `references/step-1-taste.md`.

## Rules
- No long dash character (U+2014) in files or messages.
- Never invent source. Every kept source tested this session.
- Never invent preference. Only what user said or confirmed.
- No personal names in files unless user asks (people tags come later, step 1).
- Write files in workspace folder (see SKILL.md). Create `config/` if missing.
