# Agent skills

Personal, reusable guidance for coding agents.

| Skill | Purpose |
| --- | --- |
| [proptest-praxis](skills/proptest-praxis/SKILL.md) | Build hermetic property-testing suites with independent models, small-alphabet and equivalence-pair generators, sanitizer checks, and minimal reviewable fixes. |
| [proptest-campaign](skills/proptest-campaign/SKILL.md) | Recover a repository's operating model, caller preconditions, and triage taxonomy, then write repo-specific routing and reporting sub-skills to disk. |
| [write-agent-prompts](skills/write-agent-prompts/SKILL.md) | Write guidance for capable peers using high-salience referents, conceptual breadth, structural transfer, and semantic compression. |
| [write-plans](skills/write-plans/SKILL.md) | Write plans that preserve intent, evidence, a useful method repertoire, and decision criteria across context changes. |
| [write-reports](skills/write-reports/SKILL.md) | Project technical findings, reproductions, and fixes onto a reader's decision space, contract boundaries, and reporting channel. |

The property-testing skills accompany [Property Testing with Agent Swarms](https://recursion.wtf/posts/agents-and-property-tests/). This repository holds the canonical versions; the [original gist](https://gist.github.com/inanna-malick/4e042889df0d8737794a875dda7c8f3c) remains available for existing links.

Each skill lives in its own directory under `skills/`. `SKILL.md` contains its discovery metadata and instructions; supporting references are loaded when relevant.

Give the relevant `SKILL.md` to your agent, or link its directory into your agent's skill directory (`~/.codex/skills/` for Codex, `~/.claude/skills/` for Claude Code). Once loaded, invoke these as `$proptest-praxis`, `$proptest-campaign`, `$write-agent-prompts`, `$write-plans`, or `$write-reports` (or `/proptest-campaign`, etc., in Claude Code).

Keep reusable principles in the skill and domain examples in references. Add guidance when it changes decisions; avoid accumulating instructions that merely retell one task.
