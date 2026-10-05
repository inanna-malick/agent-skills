# Worked example: a property-testing investigation

The [agent property-testing prompt](https://gist.github.com/inanna-malick/4e042889df0d8737794a875dda7c8f3c) illustrates this writing method. The objective was broad investigation of a complex repository, producing small regression-test-and-fix PRs. The agent builds machinery that searches operation histories, then investigates what it finds.

The following examples show how a conceptual cue becomes actionable guidance. They are examples of prompt construction, not a required testing checklist for other tasks.

| Referent | Concrete instruction it supports | Decision it changes |
| --- | --- | --- |
| History-dependent behavior | Reach identical logical contents through different mutations, reads, sharing, and allocation histories. | Look beyond final contents and individual methods. |
| Generator support and sampling distribution | Inspect whether operations reuse keys, overwrite with unequal values, and reach meaningful state combinations. | Improve generated behavior before increasing run volume. |
| Partial oracles and solution checking | Validate returned paths or dependency ordering independently; use a small-instance oracle to check completeness. | Exploit outputs cheaper to check than compute while tracking gaps in what the checks establish. |
| RIPR: reachability, infection, propagation, revealability | Generate histories that reach faulty code, create incorrect state, expose it through an operation, and assert on the result. | Diagnose which link is missing when a deliberate mutation survives. |
| Variant analysis | After a missed invalidation, inspect every mutation of that state and other caches maintained by the same mechanism. | Turn a finding into new investigations. |
| Exploration/exploitation | Allocate effort among new targets, broader generators, stronger checks, and longer runs according to likely information gain and cost. | Adapt the work as evidence arrives. |

An effective clause names the method and supplies the application:

> **Mutation testing as harness calibration:** temporarily omit a cache invalidation, skip a secondary-index update, or mishandle an overwrite. Check that generated histories reach the affected state and assertions detect the wrong behavior. Investigate surviving mutations, then revert the deliberate defects.

The method name gives the agent a retrieval cue. The examples anchor it to practical operations. The observation determines the next action. The final constraint keeps the experiment out of the shipped fix.

The full prompt also loads metamorphic testing, grammar-aware generation, round trips, boundary-value analysis, fault injection, failure atomicity, copy-on-write isolation, combinatorial interaction coverage, swarm testing, shrink-aware generation, counterexample generalization, and root-cause clustering. This breadth equips the agent to recognize opportunities beyond the initial leads. A concept need not have a preselected target to belong in the prompt; its role can be to make that target recognizable later.

Another important distinction was **simple and easy to validate** reference implementations. A Vec and O(n²) loops can be appropriate because the model is easier to inspect. Slowness itself is not the objective. Preserve distinctions like this when compressing guidance.

The prompt also separates the search machinery from the deliverable: independently reviewable regression tests and minimal fixes. That output constraint supports a broad, adaptive investigation without transferring the entire exploratory codebase to reviewers.
