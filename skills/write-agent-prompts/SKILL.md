---
name: write-agent-prompts
description: "Write or refine instructions for capable agents and agent swarms. Use when the deliverable is an agent prompt or reusable operating guidance, especially guidance for recognizing opportunities, choosing domain methods, and adapting an investigation."
---

# Write Agent Prompts

Write for a capable peer. Supply the conceptual tools, decision criteria, and task context that improve its judgment. Optimize for useful decisions per unit of context. Here, **agentese** means precise, information-dense technical language with enough concrete meaning to act on.

## Establish the work

Extract the objective, useful deliverables, evidence standard, available capabilities, and actual constraints from the conversation. Preserve the user's distinctions and corrections. Ask about missing information that changes the strategy; let the agent resolve ordinary implementation choices.

Separate the investigation from its handoff. A large exploratory artifact may produce a small independently verifiable result. State what the recipient must be able to inspect or reproduce, and let that shape the deliverable.

## Use high-salience referents

Use established names for relevant methodologies, techniques, and conceptual lenses as retrieval cues. A well-chosen term brings a useful body of knowledge into context; its operational clause selects the part needed here.

Connect a cue to the structure that makes it applicable, the action it suggests, and the evidence or decision it should produce. For example: **oracle independence and common-mode failures** directs an agent to inspect whether a reference model shares algorithms or assumptions with production and could agree on the same wrong answer.

Choose cues that change a likely decision. Expand unfamiliar or ambiguous acronyms once. Remove labels that add no usable distinction. Dense prose should remain technically interpretable; avoid cryptic shorthand, invented terminology, persona theatrics, and unexplained catalogs of method names.

When domain knowledge is uncertain, research candidate methods and adjacent approaches in primary sources before writing the guidance. Check that each technique is expressible with the available tools and appropriate to the task's scale.

## Teach ways of seeing

Give the agent structural recognition criteria: responsibilities, dependencies, representations, construction histories, invariants, transformations, and relationships between APIs. These let it find relevant targets whose names never appear in the prompt.

Distinguish a **lens** that changes what the agent notices from a **technique** that tells it how to investigate. History-dependent behavior is a lens; model-based stateful differential testing is a technique. Pair them when the connection is useful.

Use examples to reveal the mechanism. Make the list explicitly extensible: infer analogous targets, combine techniques, and propose approaches suggested by the code or evidence. Give the agent a basis for creativity, including how to recognize that a new idea is promising.

## Direct effort toward leverage

State the task's leverage criterion. Examples include complex behavior with a simple independent check, outputs cheaper to validate than compute, a shared mechanism behind many symptoms, or information gain relative to execution cost. Use these criteria to prioritize opportunities.

For repeated search or evaluation, have the agent build reusable machinery: generators, reference models, analyzers, scripts, or evaluation harnesses. The agent designs and improves the procedure; the resulting program explores cases. Direct attention toward what that machinery can generate, observe, and distinguish.

For swarm work, specify useful worker boundaries, deliverables, shared investigation state, and how findings are integrated. Allow the planner to adapt roles and assignments within the user's scope and available capabilities.

## Specify feedback and adaptation

Describe how evidence changes the next action: reproduce an anomaly, isolate its cause, investigate structural analogues, strengthen an insufficient check, or redirect effort. Let discoveries expand the candidate set.

Make negative evidence interpretable. Before concluding that a search found nothing, ask whether its inputs, observations, and checks could reveal the phenomenon. Specify a concrete sensitivity check when one is available.

Distinguish established findings, test or model errors, and genuine ambiguity. Give the agent ownership of technical investigation and a clear reason to involve the human: consequential uncertainty, missing organizational context, or a decision outside the delegated scope. Define continuation and completion in terms of the task's evidence, deliverables, and limits.

## Compress and review

Cut repeated instructions, process narration, generic exhortations, and explanations a capable model can reconstruct. Retain distinctions that change target selection, execution, interpretation, or review. Preserve conditions, dependencies, and failure modes when shortening sentences.

Keep human-facing motivation separate from the executable agent guidance. Let the task determine the prompt's structure; avoid imposing the same section template on every problem.

Read the draft as the agent receiving it. Check whether it supplies a productive first move, criteria for choosing among methods, observations that would change the plan, and a recognizable finished result. Check that examples support generalization and that autonomy is bounded by the actual assignment. Revise based on those decisions, rather than adding more terminology.

For worked examples of cue selection and operational grounding, read [references/proptest-example.md](references/proptest-example.md). Its testing vocabulary illustrates the method; choose domain-specific cues for each new task.
