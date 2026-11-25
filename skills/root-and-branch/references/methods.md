# Type-design and decomposition references

These sources establish the underlying type-system, boundary-parsing, and system-decomposition principles applied in [`SKILL.md`](../SKILL.md).

| Principle | Contribution to root-cause elimination | Source |
| --- | --- | --- |
| Parse, don't validate | Turn boundary checks into witness-carrying types (`Raw -> Result<Validated, Err>`) so downstream functions receive structural guarantees rather than re-checking preconditions. | [King, Parse, don't validate](https://lexi-lambda.github.io/blog/2019/11/05/parse-dont-validate/) |
| Make illegal states unrepresentable | Align type cardinality with valid domain states (`|Type| = |ValidStates|`), replacing boolean bags and parallel `Option` fields with closed sum types. | [Minsky, Effective ML Revisited](https://blog.janestreet.com/effective-ml-revisited/) |
| Information-hiding module boundaries | Decompose modules around design invariants and representation secrets rather than execution steps so representation invariants have one owner. | [Parnas, On the Criteria To Be Used in Decomposing Systems into Modules](https://dl.acm.org/doi/10.1145/361598.361623) |
| Essential vs. accidental complexity | Distinguish genuine domain requirements from compensating adapters accreted around early scaffolding before entrenching them in types. | [Brooks, No Silver Bullet — Essence and Accident in Software Engineering](https://www.cs.unc.edu/techreports/86-020.pdf) |
| Keeping secrets and single canonical paths | Avoid maintaining two subtly diverging paths or cached shadow booleans for one fact unless justified and checked against a clear recomputation spec. | [Lampson, Hints for Computer System Design](https://dl.acm.org/doi/10.1145/800217.806614) |
