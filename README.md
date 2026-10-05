# Agent skills

Personal, reusable guidance for coding agents.

| Skill | Purpose |
| --- | --- |
| [write-agent-prompts](skills/write-agent-prompts/SKILL.md) | Write instructions for capable peers using precise technical referents, structural recognition, concrete methods, and adaptive investigation. |

Each skill lives in its own directory under `skills/`. `SKILL.md` contains its discovery metadata and instructions; supporting references are loaded when relevant.

To make a skill available locally, link its directory into your agent's skill directory. For Codex, the default personal location is `~/.codex/skills/`. Invoke this skill as `$write-agent-prompts` once it is loaded.

Keep reusable principles in the skill and domain examples in references. Add guidance when it changes decisions; avoid accumulating instructions that merely retell one task.
