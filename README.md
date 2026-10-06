# Agent skills

Personal, reusable guidance for coding agents.

| Skill | Purpose |
| --- | --- |
| [proptest-praxis](skills/proptest-praxis/SKILL.md) | Build property-testing suites with independent models and operation-sequence generators; turn findings into small regression tests and fixes. |
| [write-agent-prompts](skills/write-agent-prompts/SKILL.md) | Write guidance for capable peers using high-salience referents, conceptual breadth, structural transfer, and semantic compression. |

The property-testing skill accompanies [Property Testing with Agent Swarms](https://recursion.wtf/posts/agents-and-property-tests/). This repository holds the canonical version; the [original gist](https://gist.github.com/inanna-malick/4e042889df0d8737794a875dda7c8f3c) remains available for existing links.

Each skill lives in its own directory under `skills/`. `SKILL.md` contains its discovery metadata and instructions; supporting references are loaded when relevant.

Give the relevant `SKILL.md` to your agent, or link its directory into your agent's skill directory. For Codex, the default personal location is `~/.codex/skills/`. Once loaded, invoke these as `$proptest-praxis` or `$write-agent-prompts`.

Keep reusable principles in the skill and domain examples in references. Add guidance when it changes decisions; avoid accumulating instructions that merely retell one task.
