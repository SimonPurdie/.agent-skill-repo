---
name: logic-puzzle-toolbox
description: Explicit-invocation reference toolbox for deterministic logic-puzzle generators and solvers, focused on player-facing deduction quality, generator design space, corpus diversity, and less-obvious correctness techniques. Offers design judgments and alternative lenses without prescribing an implementation.
disable-model-invocation: true
metadata:
  version: "0.4.1"
  invocation: "explicit-only"
---

# Logic Puzzle Toolbox

This skill exists mainly for puzzle-design failure modes that a capable coding agent may not notice from correctness or solver performance alone. It is **opinionated about puzzle qualities** while remaining **pluralistic about implementation and generation mechanisms**.

Core judgments:

- Correctness, uniqueness, and deterministic solvability are necessary for many puzzle games but say little about whether the solve is satisfying.
- Generator specifications often encode proxies for an intended experience. Reachability, solve length, clue-type variety, solver difficulty, or reveal pacing can all succeed while the puzzle remains shallow or tedious.
- Player effort from inference is different from effort from bookkeeping, scanning, or repeated obvious propagation. Interesting inference often creates intermediate relationships that matter again later.
- Clue syntax and deductive quality are separate. Simple clues can interact richly; elaborate comparisons, counts, or spatial relations can still function as answer delivery.
- Surface variation is not structural or experiential variation. Many clue templates or outputs can still instantiate a small number of logical recipes.
- Objectives, tie-breakers, pruning, and construction mechanisms can induce an accidental house style even when no style was requested.

These are lenses, not universal rules. Direct clues, repetition, narrow solve paths, and cleanup can all serve pacing, accessibility, reinforcement, or release.

## Choose references by the object under examination

Most tasks need only one reference. A second becomes useful when the question crosses from describing a symptom to explaining its cause.

### Generator — what can be produced, and what biases that space?

[`references/generation-and-design-space.md`](references/generation-and-design-space.md)

Relevant to generator architecture, clue selection, clue-language limits, optimization objectives, repeated construction patterns, and less-obvious formulations such as counterexample separation, hitting sets, and MUS/MCS.

### Puzzle — what does this instance ask the player to infer?

[`references/player-experience-and-deduction.md`](references/player-experience-and-deduction.md)

Relevant to clue design, intended solve experience, human-facing difficulty, progressive reveal systems, puzzles that feel clerical or shallow, and qualitative review of a clue set.

### Corpus — how does the generator behave across many outputs?

[`references/diversity-and-evaluation.md`](references/diversity-and-evaluation.md)

Relevant to repetitive or "samey" output, structural similarity, expressive range, corpus comparison, behavioural descriptors, novelty, and quality-diversity perspectives.

### Solver semantics — what does the formal model actually mean and prove?

[`references/solver-semantics-and-verification.md`](references/solver-semantics-and-verification.md)

Relevant to uniqueness semantics, symmetry and equivalence, solver-model versus puzzle-solution distinctions, assumptions and cores, global constraints, and independent verification. Standard introductions to backtracking, CSP, SAT, SMT, or MRV are intentionally omitted.

## Common routing patterns

- **A clue set feels trivial, tedious, or spoon-fed:** Puzzle.
- **A progressive reveal system has elaborate gating but weak reasoning:** Puzzle first; Generator second if revising the mechanism or specification that produces it.
- **Many generated puzzles feel alike:** Corpus first to characterize the sameness; Generator second to investigate why the search or construction process concentrates there.
- **The generator repeatedly settles on one construction recipe:** Generator first; Corpus second if measuring how strongly that recipe dominates output.
- **A uniqueness, equivalence, model-counting, or verifier result looks wrong:** Solver semantics.
- **The task is clue selection or generator formulation:** Generator first; Solver semantics second when assumptions, cores, hitting sets, or related machinery become relevant.
