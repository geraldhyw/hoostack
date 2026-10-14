---
name: ditto
description: Research a public figure or a published book from public material and generate a persona skill the user can talk to for advice (e.g. /ditto Alex Hormozi creates /alex-hormozi).
argument-hint: <person or book>
disable-model-invocation: true
---

# /ditto: create a persona

Target: **$ARGUMENTS**

Build a persona skill that lets the user talk to this person or book as if they were answering directly. The aim is to capture their **mental models, stances and voice**, not to archive everything they have ever said.

Follow the steps in order.

## 1. Resolve the target

1. If the target above is empty, ask the user who or what they want to ditto.
2. Decide if it is a **person** or a **book**. If it is unclear (e.g. a book title that is also a person's name, or several famous people share the name), ask with AskUserQuestion. Offer the likely candidates with a one-line description of each.
3. **Refuse private individuals.** Only public figures with substantial public material, or published books, are allowed. If the target does not qualify, explain why and stop.
4. Derive a kebab-case **slug**:
   - Person: full name, e.g. `alex-hormozi`, `naval-ravikant`.
   - Book: short title, e.g. `atomic-habits`, `100m-offers`.
5. If `.claude/skills/<slug>/` already exists, tell the user the persona already exists and suggest `/ditto-refresh <slug>` instead. Stop.
6. Also make sure the slug doesn't collide with an existing skill such as `ditto` or `ditto-refresh`.

Tell the user in one line what you are about to build (e.g. "Building `/alex-hormozi` from his books, interviews and posts").

## 2. Parallel research

Launch **3 `general-purpose` agents in a single message** so they run in parallel. Use the research brief below for each agent and fill in its angle.

**Angles for a person:**
- **A. Books & long-form writing:** their books, essays, blog posts, newsletters. The core frameworks and how they explain them.
- **B. Interviews, podcasts & talks:** published transcripts, show notes, detailed episode write-ups, conference talk summaries. How they reason live, stories they repeat, advice they give to specific situations.
- **C. Social posts, stances & criticism:** tweets/X threads, LinkedIn, Instagram captions quoted on the web; opinions on recurring topics; how their views changed over time; what critics say.

**Angles for a book:**
- **A. The book's ideas:** detailed summaries, chapter-by-chapter structure, key frameworks and terms.
- **B. The author on the book:** interviews and talks where the author explains, defends or extends the book.
- **C. Application & critique:** how readers apply it, common misreadings, criticisms, limits of the ideas.

### Research brief (give this to each agent, filled in)

> You are researching **<target>** (<person | book>) for angle **<A/B/C: description>**. The goal is to capture how they think so someone can later imitate their advice faithfully. Today is <date>.
>
> **Source priority**
> - Tier 1: their own words (their books, own website/blog/newsletter, their own podcast or channel pages, long-form interviews with them).
> - Tier 2: detailed summaries and major interviews by others.
> - Tier 3: general commentary and listicles. Use only to fill gaps.
>
> Web sources only: use WebSearch and WebFetch. Don't try to download videos or transcripts with other tools.
>
> **Stopping rules (hard):**
> - At most **10 searches** and **12 page fetches**.
> - Stop early once **3 sources in a row add nothing new** (no new framework, stance or voice marker).
> - You want mental models, not a complete archive.
>
> **Return** markdown with these sections. Tag every item with source numbers like `[S3]`.
> 1. **Frameworks:** name, one-paragraph explanation in your own words, when they apply it.
> 2. **Stances:** topic, their position, any change over time.
> 3. **Voice markers:** phrases they repeat, tone, sentence style, favourite examples and stories. Include direct quotes only if short (under 15 words) and exactly as found.
> 4. **Bio/context:** only what matters for understanding their advice.
> 5. **Criticisms/contradictions:** where relevant.
> 6. **Sources:** numbered list `[S#] title — URL — tier — date if known`.
> 7. **Gaps:** what you looked for but couldn't find.
>
> Paraphrase. Do not copy long passages of copyrighted text.

## 3. Coverage check

Merge the three agents' results. Check against this minimum bar:

- [ ] ≥5 core frameworks, each supported by ≥2 independent sources
- [ ] Stances on ≥5 recurring topics
- [ ] ≥10 voice markers (phrases, tone, pet examples)
- [ ] Brief bio/context

If the bar is **not** met, run **one** targeted gap-fill round: a single `general-purpose` agent with the same brief and budget, aimed only at the missing items. Then continue regardless, and record any remaining gaps in `sources.md`.

Do not run more research rounds than this.

## 4. Write the reference files

Create `.claude/skills/<slug>/references/` with four files. Follow the structure in `.claude/skills/ditto/templates/references.md`:

- `frameworks.md`
- `voice.md`
- `stances.md`
- `sources.md`, with `last_refreshed: <today YYYY-MM-DD>`

Rules:
- Paraphrase in your own words. Direct quotes only if short, exact and sourced.
- Renumber sources into one list in `sources.md` and keep the `[S#]` tags consistent across all files.
- When sources disagree or the person changed their mind, record both and note which is more recent.

## 5. Write the persona skill

Create `.claude/skills/<slug>/SKILL.md` using `.claude/skills/ditto/templates/persona-SKILL.md`. Fill in every `{{placeholder}}`:

- `description` should name the person/book and the domains they're best for, so the user knows when to use it.
- The identity paragraph is in the **first person**, as the person (or, for a book, as the author speaking within the book's ideas).
- Core beliefs: the 5–10 ideas that drive most of their advice.
- Keep SKILL.md lean. Detail belongs in `references/`.

## 6. Update the roster

Add a row to the `## Personas` table at the bottom of `CLAUDE.md`:

`| /<slug> | <who/what, one line> | <best for> | <today> |`

## 7. Report

Tell the user briefly:
- The command to use: `/<slug>`. Mention that if it doesn't show up yet, they may need to start a new session.
- What was captured: the core frameworks (names only) and main topics.
- How many sources, and how many were Tier 1.
- Known gaps, with a suggestion like `/ditto-refresh <slug> <topic>` to fill one.
- A reminder to fill in `me.md` if it still has placeholders, so advice is tailored.
