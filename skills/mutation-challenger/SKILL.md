---
name: mutation-challenger
description: "Test whether an all-green unit or property suite actually enforces a module's contract by seeding small, realistic semantic edits in a scratch copy, filtering out equivalent edits with concrete distinguishing inputs, and driving the suite to a mutation-adequate fixpoint."
---

# Mutation challenger: testing the test suite

Line and branch coverage only prove that tests *executed* a code path, never that assertions checked its result. Even an author's own manual adequacy check gravitates toward branches the author remembers testing. Act as an independent challenger to an all-green test suite: design a concrete `6–10` probe plan of small, realistic semantic edits across the module's mode, rollback, coupled-carrier, and boundary axes that compile cleanly and leave the target suite green while violating the documented contract, and drive the suite until `0` non-equivalent probes in that declared inventory survive replay.

Operate strictly inside an isolated scratch copy—or snapshot and restore exact file bytes immediately after every probe, verifying zero residual diff before finishing—so the working tree never retains a temporary adequacy edit.

**Contract-vs-suite diffing; blind-spot targeting.** Read public documentation, type signatures, caller preconditions (`Pre(f)`), and the existing test suite side by side before touching source code. Map what the suite *actually varies and asserts* rather than what test names claim: narrow string alphabets (`"[a-z0-9_-]{1,12}"`), single-step calls without state warmup, unchecked secondary outputs or in-place input preservation, tautological `is_ok()` assertions, or missing release-mode runs (`--release` / `-O2 -DNDEBUG`). Aim mutations directly at the gap between promised postconditions and exercised inputs.

**Competent-programmer and coupling mutations.** Skip trivial whole-body deletions or dead-code edits that fail on any call; probe realistic `1–3` line semantic slips that exercise **Offutt's coupling effect** across state and control boundaries:
- *Unforwarded mode or context flags:* drop or hardcode a parameter (`case_sensitive`, `strict`, `limit`, `depth`, `locale`) on a recursive self-call, fallback branch, or helper delegation.
- *Partial error-path state:* leave a counter, cache entry, index slot, or staged write updated when an operation returns `Err` or cancels mid-step.
- *Coupled-carrier drift:* update the primary collection while skipping eviction-queue, secondary-index, cached-length, or dirty-bit maintenance on overwrite, removal, or clear.
- *Boundary and precedence flips:* shift `<` vs. `<=` at capacity or chunk cutoffs (`16_384`), swap `first-wins` vs. `last-wins` duplicate-key resolution, reset an accumulator across streaming chunk boundaries, or conflate byte length with character count.

**Non-equivalence certificates; over-strong assertion discovery.** Classical syntactic mutators waste effort on **equivalent mutants**—changes to fast-path heuristics, internal capacity hints, or unspecified tie-breakers that preserve all promised behavior. Conversely, when a contract-preserving edit fails the suite because a test hardcoded an unpromised implementation detail, report that **over-strong assertion** so the oracle can be relaxed rather than treating every caught edit as progress. Never report an uncaught edit as a coverage gap without verifying **both** halves of its certificate:
1. *Suite pass-through:* the modified module compiles cleanly and the target test command exits `0` (when evaluating a newly authored property suite, run that property suite directly rather than letting existing example tests mask a gap in the property model, noting separately whether native repository tests already catch the edit).
2. *Observable contract divergence:* exhibit a concrete valid input or operation sequence `x in Pre(f)` under which the documented contract requires outcome `A` while the modified code produces `B != A` (confirming empirically with a temporary one-off check when subtle, then removing the check and restoring the source).

**General-law handoff; bounded-inventory replay.** For each verified uncaught edit, report the minimal source diff, the green test command, the concrete distinguishing input `x`, and the **missing specification or generator dimension** (`missing multi-chunk split`, `deduplicating model carrier`, `untested Err-then-retry history`) rather than asking for a hardcoded one-input regression test. When running standalone, either report those gaps concisely or strengthen the property/unit suite across the underlying equivalence class and replay the declared probe set until every non-equivalent probe is caught (`Red` on edit, `Green` on restored source); when running as a campaign subagent, write the uncaught-edit report to the assigned path and return a concise count receipt so the module's test worker widens the suite, replays all declared non-equivalent probes to `0` survivors, and states remaining untested dimensions plainly.

Use [`references/methods.md`](references/methods.md) when grounding mutation adequacy, coupling operators, and equivalent-mutant filtering.
