# PAS Quick Reference

A cheat sheet for the Prompt Architecture Standard. For the full workflow, see `SKILL.md`.

## Tier selector

| Task type | Sections to use |
|---|---|
| Simple / non-technical (write, explain, brainstorm) | Tier 1 only |
| Technical / agentic (multi-step, constraints, code/files) | Tier 1 + Tier 2 |
| High-stakes / complex (costly mistakes, auditable output) | All tiers |

## Section order (never reorder)

```
1. Role                  ┐
2. Objective             │ Tier 1 — always
3. Context               │
4. Instructions          │
5. Output Format         ┘
6. Scope                 ┐
7. Constraints           │ Tier 2 — technical/agentic
8. Inputs                │
9. Success Criteria      ┘
10. Validation & Reporting   Tier 3 — high-stakes
```

## Blank template — Tier 1 (simple prompts)

```markdown
# [Task Title]

## Role
[Who the executor acts as.]

## Objective
[The single goal in one sentence.]

## Context
[Why this exists; background needed.]

## Instructions
1. [Step]
2. [Step]

## Output Format
[Type, length, structure, tone of the deliverable.]
```

## Blank template — Full (technical / high-stakes)

```markdown
# [Task Title]

## Role
[Who the executor acts as.]

## Objective
[The single goal in one sentence.]

## Context
[Why this exists; background needed.]

## Scope
[The only thing(s) that may be touched or produced.]

## Constraints
- Do NOT [most important boundary first]
- MUST [non-negotiable requirement]

## Inputs
- [File / data / source to work from.]

## Instructions
1. [Step]
2. [Step]

## Output Format
[Exact deliverable shape.]

## Success Criteria
- [ ] [Objectively checkable condition]

## Validation & Reporting
- Before finishing, verify: [self-check].
- Report: [completion summary format].
```

## Checklist before handing off a prompt

- [ ] Objective is one unambiguous sentence.
- [ ] The executor's role is stated.
- [ ] Output format is explicit (not left to guessing).
- [ ] Constraints name the most important boundary first.
- [ ] Every specific value/name/path from the source is preserved verbatim.
- [ ] "Done" is verifiable via Success Criteria.
- [ ] Inferred or assumed details are flagged.
