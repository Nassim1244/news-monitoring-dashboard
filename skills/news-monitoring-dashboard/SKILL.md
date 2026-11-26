---
name: news-monitoring-dashboard
description: Build and run a personal news monitoring dashboard for any field (AI, cybersecurity, marketing, industry, regulation...). Interviews the user, builds a taste profile, publishes a mobile-first page with a database, schedules a recurring run that fetches sources, removes duplicates and sorts news into Signal, Watch, To try, Briefs and Noise, then learns from ratings and bucket moves. Use when the user wants a news watch, a "veille", a tech or market watch, a curated weekly or daily digest, or to resume, fix or extend an existing watch built with this skill.
license: AGPL-3.0-only
---

# News monitoring dashboard

Guide the user from zero to a working news watch in 4 steps, one session each if they prefer. Talk in the user's language, in normal warm sentences. Files stay in English unless the user picks another display language for the page.

## Paths
- **Skill files**: `references/...` and `assets/...` are relative to this skill folder: `${CLAUDE_SKILL_DIR}`. Read them from there.
- **Workspace**: the user's own folder for this watch. Default: current working directory. If it is this plugin's own repository, or not empty and unrelated, propose a new subfolder named after the watch (e.g. `cyber-watch/`). Confirm once, then use it for every step.
- Workspace layout, created as needed:

```
<workspace>/
  config/watch-settings.md   step 0
  config/sources.md          step 0
  config/MyTaste.md          step 1
  artifact/index.html        step 2
  run/recurring-run.md       step 2 (the scheduled prompt)
  run/mirror-local.md        optional add-on
```

## Route
Look at the workspace, then go to the first step not done. Tell the user where they are and what comes next.

| State | Step | Instructions |
|---|---|---|
| No `config/watch-settings.md` | 0. Interview: field, languages, cadence, sources | `references/step-0-interview.md` |
| No `config/MyTaste.md` | 1. Taste profile: categories, rules, seed ratings | `references/step-1-taste.md` |
| No `artifact/index.html` or `pageId` empty | 2. Page, database, first run, schedule | `references/step-2-build-page.md` |
| All present | 3. Recurring run (scheduled), or maintenance | `run/recurring-run.md` |

Maintenance requests on a built watch (add sources, review Watch to Signal thresholds, change cadence, retune rules): read `references/spec.md` and `references/data-model.md`, change the database and the `config/` files together, and keep the page and the run prompt consistent.

Read the step file fully before acting. Each step file says what to read first, what to write, and when to pause for the user.

## Reference
- `references/spec.md`: the method. Buckets, signal detection, learning, tag tree, token economy, page, run pipeline, sources.
- `references/data-model.md`: database collections shared by the page and the run.
- `references/packs/`: starting packs (AI, cybersecurity, marketing, industry). Tag tree, rule hints, candidate sources.
- `references/examples/ai-watch/`: complete anonymized example. Shows the expected depth of settings, taste profile and sources.
- `assets/*.template.md`: empty `config/` files and run prompt templates. Copy to the workspace, never edit in place.
- `assets/index.template.html`: reference page (mobile first, light and dark). Copied and filled in step 2.

## Needs
- Artifacts with the `db` capability, scheduled tasks, WebFetch, web search. Claude Code (desktop or web) works.
- The scheduled run does not load this skill. `run/recurring-run.md` is self-contained: keep it that way.

## Rules
- No long dash character (U+2014) in files or messages.
- Never invent sources, tags, people or preferences. Only what was tested this session, or what the user said or confirmed.
- No personal names in files unless the user asks (people tags only, step 1).
- Ask in rounds, max 4 questions each, with AskUserQuestion when available. Give a recommended option.
- Low tokens: excerpts come from feeds; Claude writes one line only for Signal, Watch, To try and Briefs.
