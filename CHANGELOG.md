# Changelog

All notable changes to this project are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.1.0] - 2026-11-26

### Added
- Screenshots of the watch page in the README (weeks, briefs, noise, favourites, share, tags, sources).
- Share button (🔗) on each full card and in the card sheet. It opens the system share menu (title, Claude's line, link) and falls back to copying the link.

## [1.0.1] - 2026-11-26

### Changed
- French display language: buckets are named Essentiel, À surveiller, À essayer, Brèves and Bruit on the page. Stored values are unchanged.
- README French section uses the French bucket names.

## [1.0.0] - 2026-11-26

First public release, as a Claude Code plugin. Extracted from a personal AI watch running weekly since November 2026.

### Added
- Claude Code plugin manifest; the repository is its own marketplace.
- `news-monitoring-dashboard` skill that routes the user through 4 steps: interview, taste profile, page and database, scheduled run.
- Generic field: topic, categories and sources come from the interview.
- Starting packs: AI, cybersecurity, marketing, industry.
- Languages: source languages (French, English or both) and display language chosen at setup. Titles and excerpts in another language are translated at write time.
- Cadence: weekly by default, daily as an option, day and time chosen at setup.
- Five buckets: Signal, Watch, To try, Briefs, Noise. Learning from ratings, favourites and bucket moves.
- Reference page template (mobile first, light and dark) with name, language and date locale placeholders.
- Self-contained recurring run prompt template, plus an optional local mirror add-on.
- Anonymized AI watch as a worked example.

[Unreleased]: https://github.com/Nassim1244/news-monitoring-dashboard/compare/v1.1.0...dev
[1.1.0]: https://github.com/Nassim1244/news-monitoring-dashboard/compare/v1.0.1...v1.1.0
[1.0.1]: https://github.com/Nassim1244/news-monitoring-dashboard/compare/v1.0.0...v1.0.1
[1.0.0]: https://github.com/Nassim1244/news-monitoring-dashboard/releases/tag/v1.0.0
