---
version: 1
updated: 2026-11-19
note: anonymized example from a real AI watch (v1.0.0)
---
# Tag model
- Items carry 1 to 4 hierarchical tags (dot-path ids). No primary tag.
- Item priority = best (lowest number) priority among its tags.
- Priority: 1 = bonus (shown first, more likely Signal / Watch / To try), 2 = neutral, 3 = penalty (Noise unless big).

# Topic tree
- id: models                          | label: Models                               | priority: 1
  - id: models.llm                    | label: LLMs / frontier releases             | priority: 1
  - id: models.world                  | label: World models                         | priority: 1 | keywords: JEPA, V-JEPA, world model
  - id: models.decision               | label: Decision models                      | priority: 1 | keywords: Jev, System One
  - id: models.open-weight            | label: Open-weight / local models           | priority: 2 | keywords: open weights, local, Llama, Qwen, Mistral
- id: agents                          | label: Agents & orchestration               | priority: 1
  - id: agents.multi-agent            | label: Multi-agent workflows, MCP           | priority: 1 | keywords: MCP, multi-agent, orchestration
  - id: agents.software-factories     | label: Agentic software factories           | priority: 1 | keywords: AI factory, spec-driven development
- id: practice                        | label: AI practice & tooling                | priority: 1
  - id: practice.claude-skills        | label: Claude skills & plugins              | priority: 1
  - id: practice.dev-cli-tools        | label: Dev / CLI tools (token savers, repos)| priority: 1 | keywords: RTK, CLI, GitHub, Docker
  - id: practice.techniques           | label: Techniques & workflows               | priority: 1 | keywords: prompting, context engineering
- id: vendors                         | label: Vendors & tools (followed)           | priority: 1
  - id: vendors.labs                  | label: AI vendors                           | priority: 1
    - id: vendors.labs.anthropic      | label: Anthropic                            | priority: 1
    - id: vendors.labs.openai         | label: OpenAI                               | priority: 1
    - id: vendors.labs.mistral        | label: Mistral                              | priority: 1
    - id: vendors.labs.google         | label: Google                               | priority: 1
    - id: vendors.labs.microsoft      | label: Microsoft                            | priority: 1
  - id: vendors.tools                 | label: Followed tools                       | priority: 1
    - id: vendors.tools.claude-code   | label: Claude Code                          | priority: 1
    - id: vendors.tools.n8n           | label: n8n                                  | priority: 1
    - id: vendors.tools.make          | label: Make                                 | priority: 1
    - id: vendors.tools.copilot-m365  | label: Microsoft Copilot / M365             | priority: 1
  - id: vendors.other                 | label: Other vendors                        | priority: 2
- id: sovereign                       | label: Sovereign solutions                  | priority: 1 | keywords: EU, France, sovereignty, SecNumCloud, OVH, Scaleway, self-hosted
- id: business                        | label: Business                             | priority: 1
  - id: business.smb-adoption         | label: AI adoption in SMEs                  | priority: 1
  - id: business.nocode               | label: No-code automation                   | priority: 1 | keywords: n8n, Make
  - id: business.economics            | label: Economics of AI                      | priority: 1
  - id: business.function             | label: Use cases by function                | priority: 1
    - id: business.function.sales-marketing   | label: Sales & marketing          | priority: 1
    - id: business.function.admin-finance-hr  | label: Admin, finance & HR        | priority: 1
    - id: business.function.customer-service  | label: Customer service           | priority: 1
    - id: business.function.operations        | label: Operations & production    | priority: 1
  - id: business.sector               | label: Use cases by sector                  | priority: 1
    - id: business.sector.industry            | label: Industry & manufacturing   | priority: 1
    - id: business.sector.construction        | label: Construction & trades      | priority: 1
    - id: business.sector.professional-services | label: Professional services     | priority: 1
    - id: business.sector.retail              | label: Retail & e-commerce        | priority: 1
- id: cyber                           | label: Cyber security                       | priority: 1
  - id: cyber.security-of-ai          | label: Security of AI                       | priority: 1 | keywords: prompt injection, agent risks
  - id: cyber.ai-attacks              | label: Real AI attacks                      | priority: 1 | keywords: breach, LLMjacking
  - id: cyber.cves                    | label: Software CVEs                        | priority: 1 | keywords: CVE, vulnerability
- id: ethics                          | label: Ethics, safety & governance          | priority: 1
  - id: ethics.ethics                 | label: Ethics                               | priority: 1
  - id: ethics.safety-research        | label: AI safety research                   | priority: 1 | keywords: alignment, interpretability
  - id: ethics.regulation             | label: Regulation                           | priority: 1 | keywords: EU AI Act
  - id: ethics.data-protection        | label: Data protection                      | priority: 1 | keywords: GDPR, RGPD
- id: foresight                       | label: Foresight                            | priority: 1
  - id: foresight.future-of-work      | label: Future of work                       | priority: 1
  - id: foresight.agi-trajectory      | label: AGI & capability trajectory          | priority: 1
  - id: foresight.societal-impact     | label: Societal impact                      | priority: 1
  - id: foresight.emerging-tech       | label: Emerging tech                        | priority: 1 | keywords: robotics, humanoid, BCI, quantum
- id: research                        | label: AI research, evals & benchmarks      | priority: 2
- id: compute                         | label: Compute & infrastructure             | priority: 3
  - id: compute.datacenters           | label: Major data center events             | priority: 1 | keywords: data center opening, closing, gigawatt, major project
- id: people                          | label: People of interest                   | priority: 1 | keywords: tag only when subject or quoted
  - id: people.person-a              | label: Person A (CEO of a European AI lab)  | priority: 1 | keywords: <name>, <company>
  - id: people.person-b              | label: Person B (world models researcher)   | priority: 1 | keywords: <name>, world models

# Signal definition
- Likely still relevant in 3 months or more (not a trend that vanishes in days or weeks).
- AND at least one of:
  - New paradigm or architecture (e.g. world models, decision models such as Jev, new agent patterns).
  - Change in what SMEs can do (capability or price change that opens new client use cases).
  - Strategic move of a major player (big partnership, product line shift, major infra / data center project).
  - Rule or standard taking effect (e.g. EU AI Act milestone, widely adopted standard such as MCP).
- Every frontier model release (GPT, Claude, Gemini, Mistral flagship) is Signal by default.

# Watch definition
- Promising but unconfirmed: early access, beta, first data point of a possible trend, policy discussions without decisions yet.
- New benchmarks that measure real-world or economic tasks go to Watch (not Noise).

# Briefs definition
- One-time events: worth knowing what happened and why, but not a trend. Never promoted to Signal.
- Displayed as title + one line "what happened, why", in a Briefs section above Noise.
- Qualifies: leadership changes at major players; major M&A, IPOs, shutdowns; major incidents and outages; landmark legal rulings and lawsuits.

# Noise definition
- Funding rounds and valuations.
- Benchmark and leaderboard claims ("new SOTA on X"). Exception: new benchmarks measuring real economic tasks = Watch.
- Incremental version bumps and minor model additions (e.g. one more model available in a tool).
- People drama and hot takes (except leadership changes at major players = Briefs).
- Humanoid / robotics prototypes and demos without real-world application.

# To try criteria
- Qualifies:
  - Claude skills, plugins, MCP servers.
  - CLI / dev tools and open-source repos (e.g. RTK, token savers).
  - Techniques and workflows (e.g. prompting, context engineering, agentic software factories, caveman skill).
  - New feature in a followed tool (vendors.tools.*) when usable now; otherwise Watch.
- Does not qualify:
  - Paid SaaS without free tier or trial.
  - Waitlist / not yet available (goes to Watch).
  - Needs heavy infra (GPU, Kubernetes).

# Dislikes
- Clickbait and listicles.
- Sponsored content and vendor marketing dressed as news.
- Doom or hype tone.
- Long videos and podcasts (Latent Space podcast kept as a trial source).

# Interests (positive keywords)
- Humanoids / robotics only for real-world applications helping people (health, defense, etc.).

# Context
- Advises small and mid-size companies on AI, based in the EU: relevance = usable with clients, explainable to SME managers, EU / GDPR aware.
- Claude power user (Claude Code, skills, plugins).
- n8n user.
- Spec-driven development crafter, AI factory maker, builds business apps on Docker.

# Seed examples
| title | url | tags | rating | bucket | why |
|---|---|---|---|---|---|
| Anthropic introduces Claude Fable 5.2 and Mythos 5.2 | https://sdtimes.com/ai/anthropic-introduces-claude-fable-5-2-mythos-5-2/ | vendors.labs.anthropic, models.llm | 👍 | Signal | Frontier release on main stack |
| Mistral releases open weights of Mistral Large 4 (1T params, EU infra) | https://af.net/realtime/mistral-ai-releases-open-weight-1-trillion-parameter-mistral-large-4/ | vendors.labs.mistral, models.open-weight, sovereign | 👍 | Signal | Frontier + open-weight + sovereign |
| Lambda plans $33.1B data center campus in Alabama | https://www.unite.ai/?p=503085 | compute.datacenters | 👍 | Signal | Major data center project |
| n8n 3.0 major release | https://docs.n8n.io/release-notes | vendors.tools.n8n, business.nocode | 👍 | Watch | Major version of a followed tool, breaking changes |
| World Labs debuts Atlas world model (early access, Sep 2026) | https://aiweekly.co/alerts/world-labs-debuts-atlas-an-omni-world-model-in-early-access | models.world | 👍 | Watch | World model paradigm, early access |
| United Airlines blocks personal AI assistants from booking flights | https://aitoolly.com/ai-news/2026-11-12 | agents.multi-agent, ethics.data-protection, foresight.societal-impact | 👍 | Watch | Early pushback of businesses against consumer agents |
| ChatGPT opens business profiles, visibility metrics and brand agents | https://www.marketingprofs.com/opinions/2026/56258/ai-update-november-13-2026-ai-news-and-views-from-the-past-week | vendors.labs.openai, business.function.sales-marketing | 👍 | Watch | AI search visibility for SMEs, beta |
| SemiAnalysis unveils GDPx agentic benchmark across 11 economic domains | https://af.net/realtime/page/263/?news_page=41 | research | 👍 | Watch | Benchmark on real economic tasks |
| Fireworks AI discloses new breach after cloud provider hack | https://bleepingcomputer.com/news/security/fireworks-ai-discloses-new-breach-after-cloud-provider-hack | cyber.ai-attacks, compute | 👍 | Watch | Real incident on AI infra |
| EU heads of state hold AI-only summit in Dublin | https://af.net/realtime/page/311/?news_page=51 | ethics.regulation, sovereign | 👍 | Watch | EU policy direction, Signal if decisions follow |
| AI in 51.6% of HN "Who is Hiring" posts, total postings lowest since 2013 | https://aiweekly.co/ai-news-today/edition/2026-11-16 | foresight.future-of-work | 👍 | Watch | Future of work data point |
| open-steps: skills making Claude Code suggest its own power tools | https://trendshift.io/repositories/351275 | practice.claude-skills, vendors.tools.claude-code | 👍 | To try | Installable Claude Code skill |
| Nvidia board reportedly preparing to replace its CEO | https://aitoolly.com/ai-news/2026-11-12 | compute | 🤷 | Briefs | One-time event: know what and why |
| Swiss startup Embodied AI unveils AIR wheeled humanoid | https://aiweekly.co/ai-news-today/edition/2026-11-16 | foresight.emerging-tech | 🤷 | Noise | Prototype, no real-world application |
| n8n adds Gemini 4 Carbon model | https://createwith.com/tool/n8n/updates/what-n8n-shipped-in-mid-november | vendors.tools.n8n, vendors.labs.google | 👎 | Noise | Incremental model addition |
