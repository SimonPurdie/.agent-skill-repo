# Generation and Design Space

This reference maps alternative ways to think about puzzle generation, especially choices that can quietly narrow what a generator produces. It does not define a preferred workflow.

## Generator formulations

Familiar families include solution-first/reductive generation, constraint-first construction, mutation of existing instances, and optimization over puzzle encodings. Less-obvious formulations that can expose different search objects include:

- **ambiguity- or counterexample-first**: alternative solutions are explicit objects to be separated by later constraints;
- **deduction-first**: an intended proof trace or reasoning structure is part of the generated object;
- **repertoire-oriented generation**: the target is a collection covering different regions of a design space rather than one optimum.

A large parameter space inside one mechanism can still map to a narrow family of logical structures.

## The clue language shapes the reachable design space

Clue variety has several layers:

- **surface variety**: wording or templates differ while expressing essentially the same relation;
- **relational variety**: clues express genuinely different relationships among puzzle entities;
- **deductive-role variety**: clues play different roles in the solve, such as setup, propagation, synthesis, bottleneck, confirmation, or cleanup.

A generator can have many clue templates but a narrow logical vocabulary if those templates repeatedly play the same role. The candidate clue language also bounds claims such as "minimal clue set": minimal within one language need not be minimal if richer relations or compound clues are available.

## Quality proxies in generator specifications

Specifications often encode properties intended to encourage a player experience without directly measuring it. Examples include clue reachability, deterministic or path-independent reveals, minimum steps between reveals, target clue counts, or solver-based difficulty.

These can be useful constraints, but they measure pacing, reachability, size, or computational structure rather than clue interaction or inferential richness. Expanding the clue vocabulary with comparisons, counts, spatial relations, or compound statements can enlarge the surface design space without enlarging the **deductive-role vocabulary**.

Generating clues from a known hidden solution is not itself a source of triviality. The important distinction is what inferential work remains after a clue is presented.

## Objective-induced style and concentration

Objectives, deterministic tie-breakers, pruning rules, clue ordering, and canonical construction choices can all become stylistic priors. A structural family that satisfies uniqueness, clue-count, difficulty, and performance targets reliably can dominate output even when no preference for that family was explicit.

Randomness is not required for this failure mode. A deterministic generator can still map much of its input space to a small number of structural patterns. Likewise, varying many parameters within one construction mechanism is different from having multiple generative mechanisms.

## Counterexamples, hitting sets, and clue sufficiency

For a fixed target solution `T`, let `U` be candidate clues that are true of `T`. An alternative solution `M` is excluded by any clue that is false in `M`:

```text
D(M) = { c in U : c is false in M }
```

Any clue set that excludes every alternative solution must intersect every such `D(M)`. This connects clue selection to hitting sets, counterexample-guided refinement, and the "unavoidable set" or "trade" terminology used in some puzzle families.

A related formulation treats candidate clues as assumptions and adds `semantic_solution != T`. If all clue assumptions make that formula unsatisfiable, **minimal unsatisfiable subset (MUS)** and **minimal correction subset (MCS)** machinery exposes sufficient clue subsets and their duals; weighted variants connect naturally to MaxSAT. This interpretation assumes `T` has separately been established as a valid semantic solution; otherwise unsatisfiability may reflect inconsistency rather than uniqueness. These are alternative formulations, not prescriptions for a generator architecture.

## Further reading and resources

- Barbara De Kegel and Mads Haahr, **Procedural Puzzle Generation: A Survey**. Broad survey of puzzle-generation methods. https://doi.org/10.1109/TG.2019.2917792
- PySAT `Hitman` — minimum/minimal hitting-set enumeration. https://pysathq.github.io/docs/html/api/examples/hitman.html
- PySAT `OptUx` — optimal MUS enumeration using implicit hitting sets. https://pysathq.github.io/docs/html/api/examples/optux.html
