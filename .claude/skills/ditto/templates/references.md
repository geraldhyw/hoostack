# Reference file templates

Each persona has four files in `.claude/skills/<slug>/references/`. Use the structures below. Every claim carries a source tag like `[S3]` that matches the numbered list in `sources.md`.

---

## frameworks.md

```markdown
# Frameworks: {{display name}}

## {{Framework name}}
**In short:** {{one sentence}}
**How it works:** {{a paragraph in your own words}}
**When I use it:** {{situations or question types it answers}}
**Example:** {{an example or story they use, paraphrased}}
**Sources:** [S1], [S4]

## {{Next framework}}
...
```

Order by importance: the ideas they return to most go first.

---

## voice.md

```markdown
# Voice: {{display name}}

## Tone
- {{e.g. blunt, high-energy, calm and philosophical}}

## Sentence style
- {{short punchy lines vs long reasoning; use of numbers, lists, analogies}}

## Signature phrases
- "{{short exact phrase, under 15 words}}" [S2]
- {{paraphrased verbal habit}} [S5]

## Favourite examples & stories
- {{story or example they repeat, one line}} [S3]

## How they handle questions
- {{e.g. reframes the question, asks for numbers first, answers with a story}}

## What they'd never say
- {{phrases or attitudes that would break character}}
```

---

## stances.md

```markdown
# Stances: {{display name}}

## {{Topic}}
**Position:** {{their view, in your words}}
**Why:** {{their reasoning}}
**Changed over time?** {{if yes, earlier vs later view with dates}}
**Sources:** [S1], [S6]

## ...

## Contradictions & criticisms
- {{where they contradict themselves or common critiques}} [S7]

## What they push back on
- {{common beliefs or requests they'd challenge}}
```

---

## sources.md

```markdown
# Sources: {{display name}}

last_refreshed: {{YYYY-MM-DD}}

## Sources
| # | Title | URL | Tier | Date |
|---|---|---|---|---|
| S1 | {{title}} | {{url}} | 1 | {{date or "unknown"}} |

## Gaps
- {{topics that were searched for but thinly covered}}

## Refresh log
- {{YYYY-MM-DD}}: created by /ditto ({{N}} sources)
```
