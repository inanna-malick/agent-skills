---
name: write-agent-prompts
description: "Write dense, concrete prompts and skill instructions for capable peer agents (agentese). Use when shaping target recognition, method selection, and adaptive work in instructions another agent will execute."
---

# Write Agent Prompts

Write for a capable peer. **Conceptual breadth:** load as many relevant, high-salience referents as possible, including adjacent methods and lenses. Shape what the agent notices, considers, and tries. Use **semantic compression**: established technical concepts plus the task-specific details that change their application.

## Task model and acceptance criteria

Recover the objective, evidence standard, deliverables, capabilities, and constraints from the conversation. Preserve user corrections as distinctions the prompt must maintain. Resolve uncertainties that change the strategy; leave ordinary implementation choices to the agent.

Define acceptance through observable artifacts: what must a recipient be able to inspect, reproduce, or decide? Separate exploratory work from its handoff; a large investigation can yield a small independently verifiable result.

## Structural abstraction and analogical transfer

Describe the relationships that make a target interesting: dependencies, representations, state transitions, invariants, transformations, or asymmetries between construction and verification. These recognition criteria should work when identifiers and domain vocabulary change.

Pair a **lens** that changes what the agent notices with a **technique** for investigating it. History-dependent behavior suggests varying construction histories; model-based stateful differential testing supplies an executable comparison.

Give examples whose causal structure supports transfer. Invite analogous targets, combinations of methods, and approaches suggested by new evidence. Where an analogy is tempting but invalid, name the missing relationship. Treat target lists as starting points for recognition and extrapolation.

## Retrieval cues and operational grounding

Survey broadly: core techniques, neighboring approaches, alternative formulations, representations, failure mechanisms, and explanatory lenses. Include directly applicable methods and plausible adjacencies that could become useful as the task develops. Group referents by shared structure, application, or relationship so the agent can combine them.

Established names serve as retrieval cues for attention, recognition, hypothesis generation, and analogical transfer. A cue can contribute before there is an immediate action to attach to it. Familiar terms may stand on their own; ground a cue where ambiguity would change its application: when a method fits, what to do here, what to observe, or how the result changes the next decision.

For example, **oracle independence and common-mode failures** directs an agent to check whether a reference model shares algorithms or assumptions with production and could agree on the same wrong answer. The referent supplies a methodology; the clause identifies a concrete failure mechanism.

Disambiguate overloaded terms and expand unfamiliar acronyms once. Avoid cryptic shorthand, invented labels, and persona theatrics. When domain knowledge is uncertain, research adjacent methods in primary sources and verify that the proposed technique is expressible with the available tools.

## Leverage and work allocation

**Value of information:** prioritize evidence that could change a consequential decision, relative to the cost of obtaining it. Express leverage in task terms: complex behavior with a simple check, outputs cheaper to validate than compute, or one mechanism behind many symptoms. Give the agent grounds for reallocating effort as these estimates change.

**Amortize repeated work:** when repeated search or evaluation dominates, have the agent build generators, reference models, analyzers, scripts, or evaluation harnesses. The program explores cases; the agent improves what it can generate, observe, and distinguish.

For swarm work, specify worker boundaries, shared investigation state, deliverables, and integration. Let the planner adapt assignments within the user's scope and available capabilities.

## Feedback and evidence

**Hypothesis testing and causal isolation:** connect observations to explanations and actions that could distinguish them. On an anomaly, specify what to reproduce, vary, or inspect. **Variant analysis:** use an established mechanism to identify related targets and expand the investigation.

**Negative evidence and sensitivity:** define what a quiet search can establish. Its inputs, observations, and checks must be capable of revealing the phenomenon. Where possible, supply a concrete sensitivity check and use failures of that check to improve the procedure.

Distinguish established findings, errors in the investigative machinery, and unresolved ambiguity. Specify continuation, completion, and human involvement through evidence, deliverables, and actual limits. Keep technical decisions with the agent; bring consequential uncertainty and missing organizational context to the human.

## Counterfactual review and behavioral evaluation

Consider removing a clause: what concept would lose salience, what analogy or hypothesis would become less available, or what decision might change? Cut duplication, unrelated associations, repeated process narration, and generic exhortations. Retain distinct, plausibly useful referents even when their eventual application is unknown. Preserve consequential conditions, dependencies, failure modes, and user distinctions.

Read the prompt as its recipient. Walk through a representative case, an analogous case with different vocabulary, and a plausible case where the method would not apply. Look for a productive first move, method selection, adaptation to evidence, and a recognizable handoff. Use observed ambiguities to revise the guidance.

When execution trials are warranted, judge resulting decisions and artifacts. An ablation that removes a cue can test its contribution; a mental walkthrough alone supports an editorial hypothesis. Avoid evaluations that merely check for expected words or headings.

Keep human-facing motivation separate from executable agent guidance. Let the task determine the prompt's structure. Put substantial conditional examples in references while keeping the criteria needed to choose an approach in the main instructions.

For worked examples, read [references/proptest-example.md](references/proptest-example.md). Its testing vocabulary illustrates the method; select cues appropriate to each new domain.
