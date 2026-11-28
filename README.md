# Agent Skills

Portable, CLI-agnostic skills for frontier coding agents (`Claude Code`, `Codex`, `Gemini`, and peer CLI runners). Rather than shipping shell wrappers or custom harnesses, each skill is written as a dense specification optimized for agent comprehension over human skimming—using exact formal-methods, type-design, and systems terms to recruit strong pretraining priors; if a phrase looks unfamiliar, ask your frontier LLM to unpack what work it is doing.

### Property Testing, Bug Elimination, and Maintainer Reporting

| Skill | Mode | Purpose |
| --- | --- | --- |
| [proptest-praxis](skills/proptest-praxis/SKILL.md) | Standalone or worker | Build referentially transparent property-testing suites using Hoare/Lamport refinement models, algebraic laws, two-tier `SmallCheck` + swarm generators, and minimal Red–Green unit regressions. |
| [mutation-challenger](skills/mutation-challenger/SKILL.md) | Standalone or challenger | Challenge an all-green unit or property suite by seeding realistic semantic edits in a scratch copy, proving non-equivalence with concrete inputs, and driving coverage to a fixpoint. |
| [proptest-campaign](skills/proptest-campaign/SKILL.md) | Multi-agent coordinator | Map a repository's API contracts, caller preconditions, and contributor policies once, unfold repository-specific sub-skills, and coordinate parallel subagents across modules, cross-module `produce -> consume` boundaries, adequacy checks, and clean-branch patches. |
| [root-and-branch](skills/root-and-branch/SKILL.md) | Standalone or campaign | Trace defects upslope in owned codebases, sweep sibling occurrences of the generative idiom, and eliminate whole bug classes via concept culling (`ΔConcepts <= 0`) and `\|Type\| == \|ValidStates\|`. |
| [write-reports](skills/write-reports/SKILL.md) | Standalone or campaign | Compress counterexamples, regression tests, and fixes onto a maintainer's decision space, API contracts, and contributor workflow. |

### Prompt and Plan Authoring

| Skill | Mode | Purpose |
| --- | --- | --- |
| [write-agent-prompts](skills/write-agent-prompts/SKILL.md) | Standalone or campaign | Write guidance and unfold context-specific skills for capable peer agents using high-salience technical referents, partial evaluation, and pragmatic compression. |
| [write-plans](skills/write-plans/SKILL.md) | Standalone | Write engineering plans that preserve intent, verified facts, a useful method repertoire, and decision criteria across context changes. |

The property-testing skills accompany [Property Testing with Agent Swarms](https://recursion.wtf/posts/agents-and-property-tests/), [Architecting Property-Testing Swarms](https://recursion.wtf/posts/architecting-property-testing-swarms/), and [Maintainer Sympathy](https://recursion.wtf/posts/maintainer-sympathy/). This repository holds the canonical versions; the [original gist](https://gist.github.com/inanna-malick/4e042889df0d8737794a875dda7c8f3c) remains available for existing links.

### Installation and Entry Points

Link `skills/*` into your agent's skill directory (`~/.claude/skills/` for Claude Code, `~/.codex/skills/` for Codex), or pass a single `SKILL.md` directly for a focused task:

- **Single module or bugfix (standalone):** Invoke `/proptest-praxis` (or `$proptest-praxis`), `/mutation-challenger`, `/root-and-branch`, or `/write-reports` directly in your checkout—no campaign workspace required.
- **Repository-wide verification (multi-agent):** Keep the `skills/` directory together and invoke `/proptest-campaign` (or `$proptest-campaign`). The coordinator maps repository contracts and contributor rules once, unfolds repo-specialized `state/skills/<bucket>/SKILL.md` writer skills via `skills/write-agent-prompts/SKILL.md`, `skills/write-reports/SKILL.md`, and `skills/root-and-branch/SKILL.md`, and dispatches `skills/proptest-praxis/SKILL.md` (plus the matching language reference under `references/`) and `skills/mutation-challenger/SKILL.md` to isolated module subagents.

Each skill lives under `skills/<name>/SKILL.md` with supporting domain or language guides in `references/` loaded only when relevant.
