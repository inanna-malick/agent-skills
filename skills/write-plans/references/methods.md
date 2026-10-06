# Planning methods

These sources supply underlying methods and useful distinctions. The skill applies them to written plans for capable agents and collaborators; select the concepts that fit the task.

| Method | Useful transfer into a written plan | Source |
| --- | --- | --- |
| Partial-order planning and causal links | Represent prerequisite relationships and actions that could disrupt them. Commit to ordering when a dependency requires it. A prose dependency map does not inherit a formal planner's soundness or completeness guarantees. | [Penberthy and Weld, UCPOP: A Sound, Complete, Partial Order Planner for ADL](https://homes.cs.washington.edu/~weld/papers/ucpop-kr92.pdf) |
| Assumption-based planning | Review an existing approach for assumptions that are both consequential and uncertain. Identify observations that should prompt revision and ways to reduce dependence on those assumptions. | [Dewar, The essence of Assumption-Based Planning](https://www.cambridge.org/core/books/abs/assumptionbased-planning/essence-of-assumptionbased-planning/B7288FCC5B72170F715532DC13C39B32); [RAND application, Appendix A](https://www.rand.org/content/dam/rand/pubs/monographs/2007/RAND_MG532.pdf) |
| Rational metareasoning and value of computation | Allocate additional planning and investigation according to their expected contribution to a consequential decision. Use qualitative judgment where numerical estimates would be invented. | [Russell, Rationality and Intelligence](https://people.eecs.berkeley.edu/~russell/research-bo.html) |
| Architecture decision records | Preserve the context, status, rationale, and consequences of significant choices so a future executor can understand when they should stand or be revisited. Use the lightweight principle without requiring a separate record for every choice. | [Nygard, Documenting Architecture Decisions](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions) |
