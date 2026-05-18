---
name: shiro
description: Force Shiro-mode — enumerate the solution space, collapse to the correct path, deliver with every alternative dismissed
---

$ARGUMENTS

Force [[shiro-mode]] on the objective above. Use when the problem is bounded — a specific bug, performance regression, refactor with measurable success criteria, or dependency selection.

1. Load the `shiro-mode` skill.
2. Run the enumeration:
   - Inputs, states, dependencies, error conditions, constraints
   - Use your host's code-navigation tools (jCodemunch `plan_turn` / `search_symbols` / `get_blast_radius` / `find_references` if available; otherwise LSP, ripgrep, or your IDE's project search — see `COMPATIBILITY.md`)
3. Build the decision tree — every branch, no skipping.
4. Calculate which paths satisfy correctness, budget, and invariants.
5. Collapse: eliminate failing paths until one remains.
6. Deliver with dismissals:
   > **Recommendation: X.**
   > - **A** dismissed because [specific reason]
   > - **B** dismissed because [specific reason]
   > - **C** dismissed because [specific reason]

The output is a recommendation no reviewer can ask "did you consider A?" of — because A is already dismissed.
