# Step 1 - Build taste profile

Use grill-me skill if available. Else same behaviour: relentless rounds of questions until each section clear.

## Talk rule
Prompt terse. Talk to user: normal full sentences, user's language. Rounds with AskUserQuestion if available. Max 4 questions per round. Give recommended option.

## Read first
- `references/spec.md` (sections 1, 5, 6, 8).
- `config/watch-settings.md`. Missing = STOP, tell user run step 0 first.
- `assets/MyTaste.template.md` = output format.
- Chosen packs in `references/packs/`.
- `references/examples/ai-watch/MyTaste.md` = depth example. Show user 2 or 3 lines of it when useful.

## Goal
Fill `config/MyTaste.md`. First run classifies well with zero rating history (cold start).

## Rounds
1. **Tree.** Start from chosen packs merged (or blank). Ask: missing branches, wrong branches, depth, priority 1/2/3 per branch, exclusions. Max ~60 tags. Labels in display language. Keywords in every source language (e.g. `cybersecurity, cybersécurité`).
2. **People** (optional). People of interest? One tag each under `people`. Tag only when person subject or quoted.
3. **Signal.** User definition, with concrete examples. Pack hints as starting proposal. "Signal by default" list (e.g. major release of main players, law taking effect).
4. **Watch, Briefs, Noise.** Same: definition + examples. Propose pack Noise list, user edits.
5. **To try.** What qualifies, what not (paid only? waitlist? heavy setup?).
6. **Dislikes + interests + context.** Formats, tones, topics disliked. Positive keywords. User context: role, sector, country, tools used. Generic wording, no name.
7. **Seed examples.** Web search 10 to 15 notable items last 4 weeks, spread across tree, in source languages. Show one by one or in batches: user rates 👍 / 🤷 / 👎 + bucket Signal / Watch / To try / Briefs / Noise + why (short). Aim: at least 1 per bucket.
8. **Confirm.** Show summary. Wait OK.

## Write
`config/MyTaste.md`, template format exactly (artifact import parses it):
- Tag line: `- id: <dot.path> | label: <label> | priority: <1-3> | keywords: a, b` (2 spaces indent per level).
- Seed table: `| title | url | tags | rating | bucket | why |`.
- Front matter `version: 1`, `updated: <today>`.

Then tell user: next step `references/step-2-build-page.md`.

## Rules
- No long dash character.
- Only write stated or confirmed preferences. Never invent.
- Stable ids: lowercase, dot path, hyphens, ASCII.
- Seed urls must be real (from search this session).
