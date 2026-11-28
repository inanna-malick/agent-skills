# Mutation-adequacy and test-evaluation references

These sources establish the competent-programmer, coupling, and equivalent-mutant principles applied in [`SKILL.md`](../SKILL.md).

| Principle | Contribution to suite evaluation | Source |
| --- | --- | --- |
| Competent-programmer hypothesis | Realistic defects sit syntactically close to the intended program; testing whether small semantic perturbations fail measures specification strength far better than line coverage. | [DeMillo, Lipton, and Sayward, Hints on Test Data Selection: Help for the Practicing Programmer](https://ieeexplore.ieee.org/document/1646911) |
| Mutation coupling effect | Test suites sensitive enough to catch simple semantic faults also detect complex multi-fault interactions at high rates when focused on state and boundary couplings. | [Offutt, Investigations of the Software Testing Coupling Effect](https://dl.acm.org/doi/10.1145/125489.125473) |
| Equivalent and subsumed mutant filtering | Require an observable distinguishing input `x in Pre(f)` before reporting an uncaught mutant so fast-path or unspecified tie-breaker edits never generate false test-gap work. | [Jia and Harman, An Analysis and Survey of the Development of Mutation Testing](https://ieeexplore.ieee.org/document/5487526); [Papadakis et al., Mutation Testing Advances: An Analysis and Survey](https://www.sciencedirect.com/science/article/pii/S0065245818300302) |
| Specification vs. coverage oracle gap | High statement or branch coverage with weak assertions misses `30–50%+` of seeded faults; mutation adequacy directly tests assertion and oracle completeness. | [Inozemtseva and Holmes, Coverage Is Not Strongly Correlated with Test Suite Effectiveness](https://dl.acm.org/doi/10.1145/2568225.2568271) |
