# News Monitoring Dashboard - Repo instructions

Claude Code plugin. The repo is its own marketplace.
- Manifests: `.claude-plugin/plugin.json`, `.claude-plugin/marketplace.json`.
- Skill: `skills/news-monitoring-dashboard/` (`SKILL.md`, `references/`, `assets/`).
- Keep `version` equal in `plugin.json`, `marketplace.json` and `CHANGELOG.md`.
- Validate before a release: `claude plugin validate .`

## Writing rules
- No long dash character (U+2014) in any file.
- No person names, emails, client names or local paths in the skill, prompts or examples.
- README: French first, then English. Keep both in sync.

## Branches
- `main`: releases only. Never commit or push to `main` directly.
- `dev`: all work happens here (or in short branches merged into `dev`).
- Release: update `CHANGELOG.md` and versions on `dev`, then PR `dev` to `main`
  merged with a merge commit (no squash, no rebase), title `chore(release): vX.Y.Z`.
  Then tag `vX.Y.Z` on that merge commit.
- Result: `main` has one merge commit per release
  (`git log --first-parent main`), `dev` keeps every atomic commit, and
  `git log --graph --all` shows two lines joined at each release.

## Git commits
Use Conventional Commits: `<type>(<scope>): <subject>`
- Types: `feat`, `fix`, `docs`, `refactor`, `chore`, `test`, `style`, `perf`, `build`, `ci`.
- Scopes: `plugin`, `skill`, `packs`, `assets`, `examples`, `readme`, `release`, or omit when global.
- Subject: imperative, lowercase, no final period, max 72 characters.
- Body (optional): what and why, wrapped at 72 characters.
- Breaking change: `!` after the scope and a `BREAKING CHANGE:` footer.
- Atomic: one logical change per commit.
- A `Co-Authored-By:` trailer is allowed (and expected for commits made with Claude).
