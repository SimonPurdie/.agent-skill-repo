---
name: semantic-refactoring
description: Use for refactors that change code structure, contracts, types, or ownership without an explicit request to change behavior.
---

# Refactoring

For refactors, separate behavior from representation.

If the task does not explicitly change an observable behavior, preserve that behavior.

Before you edit:

1. Identify the canonical target contract and whether it differs from the existing contract.
2. Find existing code that already provides relevant behavior.
3. Identify behavior that the task explicitly changes.
4. Identify implementation areas that can change or be replaced.

When you edit:

- Prefer to reuse existing code. Change or move it when this is a good fit for the new structure.
- You can rewrite an implementation when the new contract or ownership model requires it. The replacement must preserve all existing behavior that the task does not explicitly change.
- Remove genuinely obsolete names, types, and ownership boundaries.
- Do not add compatibility layers for removed concepts unless explicitly requested.
- Adapt consumers directly to the canonical new contract at the nearest seam.