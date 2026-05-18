---
name: sora
description: Force Sora-mode — cold-read the codebase and build the social/narrative frame for the objective
---

$ARGUMENTS

Force [[sora-mode]] on the objective above. Use when the problem is underspecified, the codebase is unfamiliar, or a proposal needs buy-in before the technical case will land.

1. Load the `sora-mode` skill.
2. Run the cold-read checklist:
   - What assumptions did the original author make?
   - What is this code afraid of?
   - What do `git log` thrash patterns reveal?
   - What do tests conspicuously not cover?
   - What does naming reveal about the domain model?
3. For stakeholder-facing work, build the frame:
   - Who is this for? What do they care about / fear?
   - What is the smallest demo that makes the benefit visceral?
   - What objections will land first?
4. Return the cold-read + frame as inputs to the next step (often [[shiro-mode]] or direct execution).

Sora-mode is the social and narrative layer. For the analytical layer, follow up with `/shiro`.
