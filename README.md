# Agent skills

Personal, reusable guidance for coding agents.

| Skill | Purpose |
| --- | --- |
| [proptest-praxis](skills/proptest-praxis/SKILL.md) | Build referentially transparent property-testing suites with refinement models, algebraic defect laws, small-scope generators, and minimal reviewable fixes. |
| [proptest-campaign](skills/proptest-campaign/SKILL.md) | Recover a repository's API promises, caller preconditions, and deliverable conventions, then write role-scoped sub-skills to disk for parallel model-based verification. |
| [root-and-branch](skills/root-and-branch/SKILL.md) | Trace defects upslope in owned codebases, sweep sibling occurrences of the syntactic idiom, and eliminate whole bug classes via concept deletion and `\|Type\| == \|ValidStates\|`. |
| [write-agent-prompts](skills/write-agent-prompts/SKILL.md) | Write guidance for capable peers using high-salience referents, conceptual breadth, structural transfer, and semantic compression. |
| [write-plans](skills/write-plans/SKILL.md) | Write plans that preserve intent, evidence, a useful method repertoire, and decision criteria across context changes. |
| [write-reports](skills/write-reports/SKILL.md) | Project technical findings, reproductions, and fixes onto a reader's decision space, contract boundaries, and reporting channel. |

The property-testing skills accompany [Property Testing with Agent Swarms](https://recursion.wtf/posts/agents-and-property-tests/). This repository holds the canonical versions; the [original gist](https://gist.github.com/inanna-malick/4e042889df0d8737794a875dda7c8f3c) remains available for existing links.

Each skill lives in its own directory under `skills/`. `SKILL.md` contains its discovery metadata and instructions; supporting references are loaded when relevant.

Link `skills/*` into your agent's skill directory (`~/.codex/skills/` for Codex, `~/.claude/skills/` for Claude Code), or give a single `SKILL.md` directly for standalone tasks (`$proptest-campaign` dispatches `skills/proptest-praxis/SKILL.md` to workers and unfolds `skills/write-reports/SKILL.md` and `skills/root-and-branch/SKILL.md`, so keep the `skills/` directory together when running campaigns). Once loaded, invoke these as `$proptest-praxis`, `$proptest-campaign`, `$root-and-branch`, `$write-agent-prompts`, `$write-plans`, or `$write-reports` (or `/proptest-campaign`, etc., in Claude Code).

Keep reusable principles in the skill and domain examples in references. Add guidance when it changes decisions; avoid accumulating instructions that merely retell one task.
