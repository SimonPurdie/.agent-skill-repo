# Diversity and Evaluation

This reference concerns collections of generated puzzles. Its central distinction is between **literal variety** and **structural or experiential variety**: millions of distinct outputs can still instantiate a few recognizable recipes.

## Different spaces of similarity

"Are these puzzles different?" is incomplete until the comparison space is specified. Useful views include:

- **surface / clue-vocabulary diversity**: names, values, wording, presentation, and clue templates;
- **clue-role diversity**: whether clues serve different functions such as setup, propagation, synthesis, bottleneck, or cleanup;
- **constraint or solution structure**: topology of clue-variable relationships or structure of completed solutions;
- **deductive diversity**: proof-trace motifs such as branching, convergence, bottlenecks, refutation, and reuse of intermediate facts;
- **reveal and difficulty profiles**: where information appears and where different kinds of effort occur;
- **generative-mechanism diversity**: different construction processes rather than different parameters of one process.

High diversity under one view does not imply high diversity under another.

## Expressive range and behavioural descriptors

Procedural-content-generation research uses **expressive range** to describe where a generator's outputs actually fall under chosen measurements. A **behavioural descriptor** is simply the representation used for that comparison.

Possible puzzle descriptors include clue-role histograms, canonicalized constraint skeletons, deduction depth or arity, proportion of final assignments versus derived intermediate relations, parallel reasoning and convergence, cleanup-tail length, or construction ancestry.

Descriptor choice matters: a generator can look diverse under clue counts while remaining narrow under solve topology. There is no universal feature vector that captures meaningful puzzle difference.

## Hidden sameness and canonicalization

Removing literal labels can expose repeated structure. A logic-grid puzzle might be reduced to a labelled constraint graph; another puzzle family might use a normalized geometric pattern, solution graph, or deduction trace with entity names erased.

If thousands of nominally different puzzles collapse to a few canonical skeletons, that gives a concrete account of "sameyness." Graph isomorphism, motif counts, edit distances, and trace distances are possible technical tools for this idea; the meaningful representation depends on the puzzle family.

## Reveal topology is not deduction topology

A sophisticated dependency graph governing when clues appear can coexist with a repeated direct-fact solve. Deterministic gating, all-of conditions, or multi-stage unlocks create temporal structure, not necessarily inferential interaction.

The converse also holds: a puzzle with all clues visible initially can have deep or highly interactive deduction.

## Novelty search and Quality-Diversity

**Novelty search** rewards behavioural difference rather than progress toward one objective. **Quality-Diversity (QD)** methods retain strong solutions across different regions of a chosen behaviour space. **MAP-Elites** is a prominent example: descriptor dimensions define bins or niches, and strong candidates are retained across them.

These are useful conceptual neighbours even when the generator is not evolutionary. Their main contribution here is the separation of **quality**, **difference**, and **coverage**, together with explicit attention to the representation in which difference is measured.

## Corpus comparison

Corpus-level analysis can reveal concentration that single-puzzle checks cannot. Comparing generator versions by the distribution of structural skeletons, clue roles, deduction motifs, or other meaningful descriptors can show whether apparent variety is broad or concentrated.

Three properties are worth keeping distinct: **individual puzzle quality**, **coverage/diversity of the corpus**, and **controllability**—whether desired regions of the design space can be reached intentionally. A diverse corpus can contain poor puzzles, and a coherent puzzle series can intentionally occupy a narrower style.

## Further reading and resources

- Oliver Withington and Laurissa Tokarchuk, **The Right Variety: Improving Expressive Range Analysis with Metric Selection Methods**. Emphasizes the importance of descriptor choice. https://arxiv.org/abs/2304.02366
- Jean-Baptiste Mouret and Jeff Clune, **Illuminating search spaces by mapping elites**. Introduces MAP-Elites. https://arxiv.org/abs/1504.04909
- Daniele Gravina et al., **Procedural Content Generation through Quality Diversity**. QD framing for procedural content generation. https://arxiv.org/abs/1907.04053
- Joel Lehman and Kenneth O. Stanley, **Abandoning objectives: Evolution through the search for novelty alone**. Introduces novelty-driven search. https://doi.org/10.1162/EVCO_a_00025
