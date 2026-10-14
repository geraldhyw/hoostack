# hoostack

A personal collection of [Claude Code](https://claude.com/claude-code) skills.

Each skill lives in its own folder under `.claude/skills/<name>/` with a `SKILL.md` and any templates or reference files it needs. Copy the ones you want into a project (or your user-level skills folder) and invoke them as slash commands.

## Skills

| Skill | Command | What it does |
|---|---|---|
| [ditto](.claude/skills/ditto/SKILL.md) | `/ditto <person or book>` | Researches a public figure or published book from public material and generates a persona skill you can talk to for advice (e.g. `/ditto Alex Hormozi` creates `/alex-hormozi`). |
| [ditto-refresh](.claude/skills/ditto-refresh/SKILL.md) | `/ditto-refresh <slug> [topic]` | Updates an existing persona with material published since its last refresh, or deep-dives one topic to fill a gap. |

## Installation

Skills are plain folders, so installing one means copying it to where Claude Code looks for skills:

- **Project skills:** `<your-project>/.claude/skills/`, available only in that project.
- **Personal skills:** `~/.claude/skills/`, available in every project.

For example, to add the Ditto skills to a project:

```bash
cp -R .claude/skills/ditto .claude/skills/ditto-refresh /path/to/your-project/.claude/skills/
```

Start a new Claude Code session afterwards so the skills are picked up.

