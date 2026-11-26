# Pack: AI

AI models, agents, tools and adoption in companies. Full worked example: `references/examples/ai-watch/`.

## Tag tree
- id: models                          | label: Models                               | priority: 1
  - id: models.llm                    | label: LLMs / frontier releases             | priority: 1
  - id: models.open-weight            | label: Open-weight / local models           | priority: 2 | keywords: open weights, local, modèle ouvert
  - id: models.world                  | label: World models                         | priority: 2 | keywords: world model, JEPA
- id: agents                          | label: Agents & orchestration               | priority: 1 | keywords: agent, MCP, multi-agent, agentique
- id: practice                        | label: AI practice & tooling                | priority: 1
  - id: practice.tools                | label: Tools, plugins, extensions           | priority: 1
  - id: practice.techniques           | label: Techniques & workflows               | priority: 1 | keywords: prompting, context engineering
- id: vendors                         | label: Vendors                              | priority: 1
  - id: vendors.labs                  | label: Main AI labs                         | priority: 1
  - id: vendors.tools                 | label: Followed tools                       | priority: 1
- id: business                        | label: Business & adoption                  | priority: 1
  - id: business.adoption             | label: Adoption in companies                | priority: 1
  - id: business.function             | label: Use cases by function                | priority: 1
  - id: business.sector               | label: Use cases by sector                  | priority: 2
  - id: business.economics            | label: Economics of AI                      | priority: 2
- id: security                        | label: AI security                          | priority: 1 | keywords: prompt injection, sécurité IA
- id: governance                      | label: Ethics, safety & regulation          | priority: 1 | keywords: AI Act, RGPD, GDPR, alignment
- id: sovereign                       | label: Sovereign solutions                  | priority: 2 | keywords: souveraineté, sovereignty, EU, self-hosted
- id: research                        | label: Research, evals & benchmarks         | priority: 2
- id: compute                         | label: Compute & infrastructure             | priority: 3
  - id: compute.datacenters           | label: Major data center projects           | priority: 1

## Bucket hints
- Signal by default: frontier model release of a main lab; regulation milestone taking effect; standard widely adopted.
- Watch: early access, beta, new benchmark measuring real economic tasks.
- To try: plugins, skills, MCP servers, open-source tools, techniques usable now; not waitlists, not paid-only.
- Briefs: leadership change at a major lab, major acquisition, major outage or breach, landmark lawsuit.
- Noise: funding rounds, "new SOTA" claims, minor version bumps, hot takes, robot demos with no real use.

## Candidate sources
Tested November 2026 (EN):

| name | lang | access | url | role |
|---|---|---|---|---|
| OpenAI News | en | rss | https://openai.com/news/rss.xml | item |
| Anthropic News | en | html | https://www.anthropic.com/news | item |
| Google DeepMind | en | rss | https://deepmind.google/blog/rss.xml | item |
| Hugging Face Blog | en | rss | https://huggingface.co/blog/feed.xml | item |
| MIT Technology Review AI | en | rss | https://www.technologyreview.com/topic/artificial-intelligence/feed/ | item |
| TechCrunch AI | en | rss | https://techcrunch.com/category/artificial-intelligence/feed/ | item |
| Simon Willison | en | atom | https://simonwillison.net/atom/everything/ | item |
| Import AI | en | rss | https://importai.substack.com/feed | item |
| One Useful Thing | en | rss | https://www.oneusefulthing.org/feed | item |
| HF Daily Papers | en | json | https://huggingface.co/api/daily_papers | item |
| TLDR AI | en | rss | https://tldr.tech/api/rss/ai | crosscheck |
| Last Week in AI | en | rss | https://lastweekin.ai/feed | crosscheck |
| Hacker News (100+ pts) | en | rss | https://hnrss.org/newest?q=AI+OR+LLM&points=100 | item |

Not tested (FR), step 0 checks them:

| name | lang | site | role |
|---|---|---|---|
| ActuIA | fr | actuia.com | item |
| Le Monde Informatique (IA) | fr | lemondeinformatique.fr | item |
| LeMagIT (IA) | fr | lemagit.fr | item |
| Mistral AI news | fr/en | mistral.ai/news | item |
| CNIL (IA) | fr | cnil.fr | item |
| Numerama (IA) | fr | numerama.com | item |
