---
name: ditto-refresh
description: Update an existing persona with material published since its last refresh, or deep-dive a specific topic to fill a gap (e.g. /ditto-refresh alex-hormozi hiring).
argument-hint: <slug> [topic]
disable-model-invocation: true
---

# /ditto-refresh: update a persona

Arguments: **$ARGUMENTS**

The first word is the persona **slug**. Anything after it is an optional **topic**.

## 1. Validate

1. If no slug was given, or `.claude/skills/<slug>/references/sources.md` doesn't exist, list the available personas (folders in `.claude/skills/` other than `ditto` and `ditto-refresh`) and ask which one to refresh.
2. Read the persona's `SKILL.md` and all four files in `references/`, so you know what's already captured and don't duplicate it.

## 2. Pick the mode

- **Recency refresh** (no topic): read `last_refreshed` from `sources.md`. Research only content published **after** that date. Launch **3 `general-purpose` agents in one message**, one per angle used by `/ditto` (see `.claude/skills/ditto/SKILL.md` step 2), each told the cutoff date.
- **Topic deep-dive** (topic given): research that topic regardless of date. Launch **1 `general-purpose` agent** focused on how this person thinks about the topic.

Use the research brief, source tiers and **stopping rules** from `.claude/skills/ditto/SKILL.md` step 2 unchanged: at most 10 searches and 12 fetches per agent, and stop after 3 sources in a row add nothing new. Also give the agents a short list of what's already captured, so they look for **new** material only.

No gap-fill round. If little new material turns up, that's a valid result.

## 3. Merge

- Append new sources to `sources.md`, continuing the numbering (`S#`). Never renumber existing sources.
- Add new frameworks, stances and voice markers to the matching files. Revise existing entries if new material sharpens them.
- If a stance changed, keep the old one and record the change under **Changed over time?** with dates.
- Remove topics from **Gaps** that are now covered, and add any new gaps.
- Update the **What I believe** section of the persona's `SKILL.md` only if a core belief meaningfully changed.
- Recency refresh: set `last_refreshed` to today. Topic deep-dive: leave `last_refreshed` alone, since it didn't check all recent material.
- Add a line to the **Refresh log** in `sources.md`: `YYYY-MM-DD: <recency refresh | deep-dive: topic> (+N sources)`.
- If the project's `CLAUDE.md` has a `## Personas` table with a row for this persona, update it (the "Best for" column if it grew, and "Last refreshed" for a recency refresh). Otherwise skip this.

## 4. Report

Tell the user briefly: how many new sources, what was added or changed (frameworks, stances, beliefs), any stance changes, and remaining gaps.
