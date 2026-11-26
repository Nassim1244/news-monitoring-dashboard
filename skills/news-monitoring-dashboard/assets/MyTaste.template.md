---
version: 1
updated: YYYY-MM-DD
---
# Tag model
- Items carry 1 to 4 hierarchical tags (dot-path ids). No primary tag.
- Item priority = best (lowest number) priority among its tags.
- Priority: 1 = bonus (shown first, more likely Signal / Watch / To try), 2 = neutral, 3 = penalty (Noise unless big).

# Topic tree
<!-- One line per tag. Indent 2 spaces per level. Fields: id (stable dot path), label (in display language), priority (1 to 3), optional keywords (in every source language), optional excluded: true -->
- id: example                         | label: Example branch                       | priority: 1
  - id: example.child                 | label: Example child                        | priority: 2 | keywords: word, mot

# Signal definition
- Likely still relevant in 3 months or more.
- AND at least one of:
  - 
- Signal by default:
  - 

# Watch definition
- Promising but unconfirmed: early access, beta, first data point of a possible trend, discussions without decisions yet.
- 

# Briefs definition
- One-time events: worth knowing what happened and why, but not a trend. Never promoted to Signal.
- Qualifies: 

# Noise definition
- 

# To try criteria
- Qualifies:
  - 
- Does not qualify:
  - 

# Dislikes
- 

# Interests (positive keywords)
- 

# Context
<!-- Who the user is, what they do with this watch. Generic description, no personal names. -->
- 

# Seed examples
| title | url | tags | rating | bucket | why |
|---|---|---|---|---|---|
