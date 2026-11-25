---
name: root-and-branch
description: "Trace defects upslope to their shared architectural cause in owned codebases, sweep sibling occurrences of the generative idiom, and eliminate whole bug classes via concept deletion, algebraic type unrepresentability (|Type| == |ValidStates|), and standard-crate replacement."
---

# Root and branch: pulling whole bug classes in owned codebases

Minimal five-line patches optimize for stranger-maintained repositories where reviewer attention is scarce and internal types are someone else's territory. In a codebase your own team maintains—especially at agentic development velocity, where thousands of lines land quickly while compilers and property suites verify wide structural changes at $O(1)$ marginal cost—patching one symptom at a time accretes boolean flags, compensating adapters, and duplicate wrappers. Treat a bug in owned code as **placer gold washing down from an upslope vein**: trace the vein to its source, pan the workspace for the habit that produced it, and pull the whole bug class out by the roots.

Use this skill standalone when fixing bugs, reviewing changes, or refactoring an owned repository, or alongside [`proptest-praxis`](../proptest-praxis/SKILL.md) and [`proptest-campaign`](../proptest-campaign/SKILL.md) when resolving divergences in owned mode.

## 1. Walk upslope and pan the workspace

Whenever a test failure, property counterexample, or review catch surfaces at `file_A:line_k`, trace the mechanism before writing a patch:

- **Follow `produce -> representation -> consume` end to end:** step above the failing helper to see what constructs the value and what consumes it across module boundaries. Look specifically for **internal `stringify -> reparse` hops**—formatting a structured identifier, path, query, or error enum into a raw `String` only to `.contains(...)`, `strip_prefix(...)`, or re-parse it upstairs—and split state where two structs, maps, or caches track the same logical fact independently.
- **Pan the generative syntactic habit (`rg` the verb, not the noun):** whoever wrote a fragile pattern once usually wrote it across sibling files. Extract the syntactic idiom and sweep the workspace:
  - *Silent error or fallback masking:* `unwrap_or_default()`, `.ok()`, `let _ =`, or catch-all fallback returns that swallow invariant breaks;
  - *Stringly-typed dispatch and case/prefix sniffing:* `.to_lowercase()`, `.to_ascii_lowercase()`, `eq_ignore_ascii_case`, `.contains(`, or `strip_prefix(` over structured domain values;
  - *Exhaustiveness holes:* `_ =>` wildcards on internal enums that mute compiler exhaustiveness checks (`rustc E0063`) when new variants are added;
  - *Boolean/Option bags and shadow verdict bits:* multi-`bool` state structs, `Option<bool>`, or stored `is_valid: bool` / `complete: bool` fields that can drift out of sync with the underlying value;
  - *Duplicate pocket wrappers:* repeated mini-parsers, quote/fence strippers, connection helpers, or retry loops across sibling crates.
- **Check module mass and dependency arrows:** inspect large multi-responsibility files (`wc -l`) where distinct strata accumulated in one place, and check `Cargo.toml` / package manifests for inverted dependency arrows that forced downstream modules to duplicate types rather than importing one canonical definition.

## 2. Resolve at the highest applicable rung

Eliminate the root-cause equivalence class at the highest rung that fits the codebase:

1. **Rung 0 — Premise check ("three-adapter rule"):** before sculpting clean types around an awkward interaction, check 10,000-foot product reality—is this behavior something users genuinely want, or early prototype scaffolding that accreted green tests? When a mechanism demands its **third compensating adapter or special-case flag**, pause and ask the human owner one concise two-line question so you delete accidental complexity rather than memorializing it in types.
2. **Rung 1 — Concept and duplicate-stratum cull (`ΔConcepts <= 0`):** aim for structural fixes that leave the system with *fewer* concepts than before even when test coverage grows. Delete dead `v1` fallback paths, merge parallel `foo` and `foo_v2` strata into one canonical path, and invert tangled package arrows so duplicate helpers disappear.
3. **Rung 2 — Algebraic type unrepresentability (`|Type| == |ValidStates|` — parse, don't validate):** tighten types so invalid or out-of-envelope states cannot be constructed. Parse raw inputs into validated witness newtypes (`ValidatedQuery`, `NormalizedPath`, `NonZeroUsize`) once at the system boundary (`#[serde(try_from)]`), replace multi-`bool` bags and `Option<bool>` with closed sum types encoding legal state transitions, compute summary verdicts via methods rather than stored shadow booleans, and omit `_ =>` wildcards on internal enums so the compiler routes future variants to every call site automatically.
4. **Rung 3 — Mature crate or visitor swap:** replace 85%-complete hand-rolled scanners, URL/glob/query formatters, and ad-hoc recursive traversals with standard crates or visitor traits already in the dependency graph (`sqlparser::ast::Visitor`, `url`, `serde`, `tree-sitter`, typed AST builders).

## 3. Verify across the equivalence class

Write a deterministic regression test at the public module boundary (plus a [`proptest-praxis`](../proptest-praxis/SKILL.md) law when state transitions or parsers are involved) covering both the original site and any sibling call sites uncovered by the workspace sweep. Confirm the test fails on the baseline revision and passes across debug and `--release` builds after the root fix.

Use [`references/methods.md`](references/methods.md) when grounding these type-design and decomposition principles.
