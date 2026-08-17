# Solver Semantics and Verification

This reference omits standard introductions to backtracking, CSP, SAT, SMT, MRV, and ordinary unit testing. It focuses on puzzle-specific traps and solver facilities that are easier to overlook.

## What counts as a distinct solution?

Solver encodings often contain auxiliary variables—reification or Tseitin variables, channeling variables, flow/reachability state, or other bookkeeping—that are not part of the player-visible answer. Two complete solver models can differ on auxiliaries while representing the same puzzle solution.

This is the distinction between **model uniqueness** and **projected/semantic uniqueness**. Model enumeration, model counting, and second-solution checks may need to project onto the variables that define the puzzle answer.

Symmetry adds another layer. **Solver symmetry breaking**, **puzzle equivalence** (for example interchangeable labels or rotations), and **aesthetic symmetry** of the presented clues are different concepts. A uniqueness check can answer the wrong question if these are conflated.

## Uniqueness needs existence and no alternative

For a known semantic target `T`, uniqueness has two parts:

```text
SAT(base rules
    AND selected clues
    AND semantic_solution = T)

UNSAT(base rules
      AND selected clues
      AND semantic_solution != T)
```

The first establishes that `T` is actually a valid solution. The second establishes that no inequivalent semantic alternative exists under the chosen projection and equivalence relation.

Without the first check, an inconsistent puzzle—or an encoding bug that rejects `T`—can also make the second query unsatisfiable. That is a vacuous uniqueness result, not evidence of a unique valid puzzle.

This two-part view also exposes incremental solving, assumptions, cores, and subset-minimization machinery.

## Assumptions, cores, MUS/MCS, and hitting sets

SAT and SMT solvers commonly support temporary **assumption literals** over a persistent base problem. Candidate clues represented as assumptions make many related puzzle variants cheap to query and can expose **unsatisfiable cores**—subsets of assumptions already sufficient for contradiction. A core is not necessarily minimal.

Related terms worth recognizing:

- **MUS**: a minimal unsatisfiable subset;
- **MCS**: a minimal correction subset whose removal restores satisfiability;
- **hitting set**: a set intersecting every member of a family of sets;
- **MaxSAT**: optimization over soft constraints under hard constraints.

"Minimal" means no element can be removed; it does not mean minimum cardinality or minimum cost. MUS/MCS and hitting-set dualities appear in clue sufficiency, counterexample exclusion, weighted clue selection, and human-readable explanation work.

## Global constraints as an idea catalogue

Constraint-programming libraries contain named abstractions for structures puzzle implementations often encode manually: `all_different`, cardinality, extensional `table` constraints, `regular`/automaton constraints, circuits, paths, connectivity, flows, spanning trees, and sequencing relations.

These catalogues can be useful even when the final implementation uses SAT, SMT, ILP, or bespoke search because they provide vocabulary for known structures and propagation techniques. Logically redundant implied constraints can also improve propagation; redundancy does not imply computational uselessness.

## Independent verification

A generator and verifier can share the same modelling bug if they reuse the same encoding. Stronger checks can include an independently implemented verifier, a second encoding or solver paradigm, differential testing, exhaustive enumeration on reduced cases, property/metamorphic tests, and regression corpora containing known edge cases.

Diagnostic prose is another possible source of error. Claims such as "this clue entails X" or generated reveal-plan walkthroughs can disagree with the encoded instance, so important audit claims can be checked against the same formal semantics used for validation.

## Useful resources

- **MiniZinc global constraint library** — catalogue of high-level modelling structures. https://docs.minizinc.dev/en/stable/lib-globals.html
- **Global Constraint Catalog** — extensive dictionary of named global constraints and structural patterns. https://sofdem.github.io/gccat/
- **PySAT** — SAT toolkit with assumptions and implementations/examples for hitting sets, MUS/MCS, MaxSAT, and model enumeration. https://pysathq.github.io/docs/html/
- **Z3 Guide** — SMT documentation including assumptions, cores, models, and optimization. https://microsoft.github.io/z3guide/
- **Conjure** — high-level constraint modelling and automated refinement, useful for seeing alternative representations. https://conjure.readthedocs.io/
- **XCSP3** — constraint-programming format and benchmark/model ecosystem. https://xcsp.org/
