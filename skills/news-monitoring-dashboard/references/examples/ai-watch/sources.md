---
generated: 2026-11-19
note: anonymized example, tested via WebFetch in November 2026
---
# Sources

Columns: id | name | type | lang | access | url | role | on | notes. Role: item (shown), crosscheck (corroboration only, not shown), totry (To try candidates). Edit only `on` (yes/no), `role` or `notes`, or add a row with a new id.

| id | name | type | lang | access | url | role | on | notes |
|---|---|---|---|---|---|---|---|---|
| openai | OpenAI News | Lab | en | rss | https://openai.com/news/rss.xml | item | yes | Heavy feed |
| anthropic | Anthropic News | Lab | en | html | https://www.anthropic.com/news | item | yes | |
| deepmind | Google DeepMind | Lab | en | rss | https://deepmind.google/blog/rss.xml | item | yes | |
| google-ai | Google AI Blog | Lab | en | rss | https://blog.google/technology/ai/rss/ | item | yes | |
| microsoft-ai | Microsoft AI Blog | Lab | en | html | https://www.microsoft.com/en-us/ai/blog/ | item | yes | |
| huggingface | Hugging Face Blog | Lab/dev | en | rss | https://huggingface.co/blog/feed.xml | item | yes | |
| mit-tr | MIT Technology Review AI | News | en | rss | https://www.technologyreview.com/topic/artificial-intelligence/feed/ | item | yes | |
| techcrunch | TechCrunch AI | News | en | rss | https://techcrunch.com/category/artificial-intelligence/feed/ | item | yes | |
| ai-news | AI News | News | en | html | https://www.artificialintelligence-news.com/ | item | yes | Page, not feed |
| simon-willison | Simon Willison | Blog | en | atom | https://simonwillison.net/atom/everything/ | item | yes | |
| import-ai | Import AI | Newsletter | en | rss | https://importai.substack.com/feed | item | yes | Substack full text |
| one-useful-thing | One Useful Thing | Newsletter | en | rss | https://www.oneusefulthing.org/feed | item | yes | Substack full text |
| interconnects | Interconnects | Newsletter | en | rss | https://www.interconnects.ai/feed | item | yes | Substack full text |
| ahead-of-ai | Ahead of AI | Newsletter | en | rss | https://magazine.sebastianraschka.com/feed | item | yes | Very heavy |
| the-batch | The Batch | Newsletter | en | html | https://www.deeplearning.ai/the-batch/ | crosscheck | yes | |
| tldr-ai | TLDR AI | Digest | en | rss | https://tldr.tech/api/rss/ai | crosscheck | yes | Light |
| rundown-ai | The Rundown AI | Digest | en | rss | https://rss.beehiiv.com/feeds/2R3C6Bt5wj.xml | crosscheck | yes | |
| latent-space | Latent Space / AINews | Digest | en | rss | https://www.latent.space/feed | crosscheck | yes | |
| last-week-in-ai | Last Week in AI | Digest | en | rss | https://lastweekin.ai/feed | crosscheck | yes | |
| hf-papers | HF Daily Papers | Papers | en | json | https://huggingface.co/api/daily_papers | item | yes | Curated |
| arxiv-csai | arXiv cs.AI | Papers | en | rss | https://rss.arxiv.org/rss/cs.AI | item | no | Off by default: noisy, about 50 a day |
| hn-100 | Hacker News (100+ pts) | Community | en | rss | https://hnrss.org/newest?q=AI+OR+LLM&points=100 | item | yes | Light |
| github-trending | GitHub Trending | Code | en | html | https://github.com/trending | totry | yes | Not AI-filtered |
| latent-space-podcast | Latent Space podcast | Podcast | en | rss | https://podcast.latent.space/feed.xml | item | yes | Trial: ratings decide |

# Excluded or deferred (not in DB, see `references/spec.md`, section 12)
VentureBeat AI (stale), smol.ai news (stale), MarkTechPost (feed fails), The Decoder and Practical AI (robots), The Verge, Ars Technica, Wired, Reddit, YouTube, LinkedIn, X (blocked or login: browser later).
