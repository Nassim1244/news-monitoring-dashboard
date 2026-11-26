# Pack: Cybersecurity

Threats, vulnerabilities, incidents, regulation and defense practice.

## Tag tree
- id: threats                         | label: Threats & attacks                    | priority: 1
  - id: threats.ransomware            | label: Ransomware                           | priority: 1 | keywords: ransomware, rançongiciel
  - id: threats.phishing              | label: Phishing & social engineering        | priority: 1 | keywords: phishing, hameçonnage
  - id: threats.supply-chain          | label: Supply chain attacks                 | priority: 1
  - id: threats.state                 | label: State-sponsored groups               | priority: 2 | keywords: APT
- id: vulns                           | label: Vulnerabilities                      | priority: 1
  - id: vulns.critical                | label: Critical, exploited in the wild      | priority: 1 | keywords: CVE, zero-day, KEV
  - id: vulns.other                   | label: Other CVEs                           | priority: 3
- id: incidents                       | label: Major incidents & breaches           | priority: 1 | keywords: breach, fuite de données, data leak
- id: defense                         | label: Defense practice                     | priority: 1
  - id: defense.tools                 | label: Tools & open source                  | priority: 1
  - id: defense.identity              | label: Identity & access                    | priority: 1 | keywords: MFA, passkeys, IAM
  - id: defense.cloud                 | label: Cloud security                       | priority: 2
  - id: defense.awareness             | label: Awareness & training                 | priority: 2
- id: regulation                      | label: Regulation & standards               | priority: 1 | keywords: NIS2, DORA, CRA, ISO 27001, RGPD
- id: ai-security                     | label: AI and security                      | priority: 1 | keywords: prompt injection, AI attacks
- id: smb                             | label: Small business security              | priority: 1 | keywords: PME, TPE, SMB
- id: market                          | label: Security market & vendors            | priority: 3

## Bucket hints
- Signal by default: regulation milestone taking effect (e.g. NIS2 deadlines); new attack technique spreading across sectors; major vendor strategy shift.
- Watch: new threat group or technique seen once; draft regulation.
- To try: free tools, hardening guides, checklists, official templates.
- Briefs: major breach at a known company, major outage, arrest or takedown of a big group.
- Noise: vendor reports dressed as news, funding rounds, minor CVEs, fear-based headlines.

## Candidate sources (not tested)
| name | lang | site | role |
|---|---|---|---|
| CISA advisories | en | cisa.gov | item |
| Krebs on Security | en | krebsonsecurity.com | item |
| BleepingComputer | en | bleepingcomputer.com | item |
| The Hacker News | en | thehackernews.com | item |
| SANS Internet Storm Center | en | isc.sans.edu | item |
| Dark Reading | en | darkreading.com | item |
| tl;dr sec | en | tldrsec.com | crosscheck |
| CERT-FR | fr | cert.ssi.gouv.fr | item |
| ANSSI actualités | fr | cyber.gouv.fr | item |
| Cybermalveillance.gouv.fr | fr | cybermalveillance.gouv.fr | item |
| LeMagIT (sécurité) | fr | lemagit.fr | item |
| ZATAZ | fr | zataz.com | item |
| GitHub Trending (security) | en | github.com/trending | totry |
