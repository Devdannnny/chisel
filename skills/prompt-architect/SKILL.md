---
name: prompt-architect
description: Convert any rough, casual, or unstructured human prompt into a clean, professional, well-architected prompt. Use this whenever a user pastes a messy instruction, a brain-dump, a bullet list, or a vague request and wants it rewritten as a structured prompt — for an AI agent, a coding assistant, a writing task, or a teammate. Trigger this whenever the user says things like "structure this prompt", "make this a proper prompt", "turn this into a format", "rewrite this professionally", "clean up this instruction", or pastes a wall of text that is clearly meant to be an instruction. Also use it proactively when someone is about to hand a low-quality prompt to another model and would benefit from a tighter version.
---

# Prompt Architect

This skill turns a rough human prompt into a professionally structured one using a single, scalable framework called the **Prompt Architecture Standard (PAS)**.

The same framework works for a non-technical request ("write me a birthday speech") and a complex technical/agentic task ("refactor this service, don't touch the API"). It scales by adding or dropping sections — the structure never changes, only the depth.

## The job

Given a rough prompt, produce a clean structured prompt. Do **not** execute the task in the rough prompt — restructure it. The deliverable is *a better prompt*, not the answer to it.

## Workflow

Follow these steps in order.

### 1. Extract intent
Read the rough prompt fully. Identify, in your own notes: the real goal, the implied audience/executor (a human? an AI agent? a coding tool?), any constraints stated or implied, and any concrete details that must be preserved verbatim (file names, values, names, URLs, exact wording).

### 2. Classify the task
Pick one:
- **Simple / non-technical** — a single creative or informational ask (write, summarize, explain, brainstorm). Use **Tier 1** sections.
- **Technical / agentic** — a multi-step task with constraints, a defined target, or code/file changes. Use **Tier 1 + Tier 2** sections.
- **High-stakes / complex** — agentic work where mistakes are costly, scope creep is a risk, or output must be auditable. Use **all tiers**.

When in doubt, go one tier higher — extra structure rarely hurts.

### 3. Map content into sections
Place every piece of the rough prompt into the correct PAS section (defined below). Nothing from the original should be silently dropped. If the user listed specific values, statuses, or replacements, preserve them exactly inside the relevant section.

### 4. Fill gaps and flag assumptions
Rough prompts are usually missing a role, an explicit output format, or success criteria. Add sensible defaults, but never invent constraints the user didn't imply. Mark anything you inferred so the user can correct it.

### 5. Output
Return the structured prompt in a code block (so it is copy-pasteable as a single unit), then a short **"Notes"** section listing what you inferred, what you added, and any gaps the user should fill in.

---

## The Prompt Architecture Standard (PAS)

Ten sections, grouped into three tiers. Always use this order. Use only the sections the task needs.

### Tier 1 — Essential (every prompt)

1. **Role** — Who the executor should act as. Sets expertise and perspective.
   > *"You are a senior backend engineer."* / *"You are an experienced speechwriter."*

2. **Objective** — The single goal, in one sentence. If you can't state it in one sentence, the task isn't scoped yet.
   > *"Refactor the payment module to remove duplicated validation logic."*

3. **Context** — Background and the *why*. Situational facts the executor needs but the objective doesn't carry. Keeps the executor from guessing.

4. **Instructions** — The ordered steps or the substance of the request. Numbered when sequence matters; imperative voice ("Do X", not "You should do X").

5. **Output Format** — The exact shape of the deliverable: file type, length, structure, headings, tone. Vague prompts almost always omit this — it is the highest-leverage section to add.

### Tier 2 — Add for technical / agentic tasks

6. **Scope** — What is explicitly in bounds. One sentence naming the only thing(s) the executor may touch or produce.

7. **Constraints** — Hard boundaries as a "Do NOT" / "MUST" list. The most reliable guardrail against scope creep. Place the single most important constraint first *and* restate it last.

8. **Inputs** — The material the executor is given to work on (files, data, prior text), and where to find it.

9. **Success Criteria** — A checklist defining "done correctly". Turns a subjective task into a verifiable one. Each item should be objectively checkable.

### Tier 3 — Add for high-stakes / complex work

10. **Validation & Reporting** — A self-check the executor runs before finishing (what to verify, what to search for), plus the exact format of the completion report (what changed, what was skipped, remaining issues). This closes the loop and makes the result auditable.

**Optional add-ons** (insert near the relevant section when useful):
- **Examples** — one or two input→output pairs. Few-shot examples beat lengthy description for format-sensitive tasks.
- **Tone & Style** — for writing tasks, specify voice, register, and audience.
- **Edge Cases** — how to handle ambiguity or unexpected input.

---

## Output template

Use this exact skeleton. Drop unused sections; never reorder.

```markdown
# [Task Title]

## Role
[Who the executor should act as.]

## Objective
[The single goal in one sentence.]

## Context
[Why this task exists; background the executor needs.]

## Scope
[The only thing(s) that may be touched/produced. — technical tasks]

## Constraints
- Do NOT [hard boundary — most important one first]
- MUST [non-negotiable requirement]
- [...]

## Inputs
- [File / data / source the executor works from.]

## Instructions
1. [First step]
2. [Second step]
3. [...]

## Output Format
[Exact deliverable shape: type, length, structure, tone.]

## Success Criteria
- [ ] [Objectively checkable condition]
- [ ] [...]

## Validation & Reporting
- Before finishing, verify: [self-check].
- Report: [exact format of the completion summary].

## Notes / Assumptions
[Anything inferred or added — flagged for the user to confirm. — only in the structured output if useful; otherwise put outside the code block.]
```

---

## Worked example

**Input (rough prompt):**
> "hey can u fix my python script its too slow and also the variable names r bad. dont change what it does just make it faster and cleaner. also add comments"

**Output (structured prompt):**

```markdown
# Optimize and Clean Up Python Script

## Role
You are a senior Python engineer focused on performance and readable code.

## Objective
Improve the runtime performance and readability of the provided Python script without changing its behavior.

## Scope
The single attached Python script. Do not create new files.

## Constraints
- Do NOT change the script's observable behavior, inputs, or outputs.
- Do NOT add external dependencies unless strictly necessary (flag if so).
- MUST keep the script runnable as a drop-in replacement.

## Inputs
- The Python script provided in this conversation.

## Instructions
1. Profile or reason about the script to locate performance bottlenecks.
2. Optimize the slow sections while preserving identical behavior.
3. Rename unclear variables to descriptive, PEP 8-compliant names.
4. Add concise comments explaining non-obvious logic.

## Output Format
The full revised script in a single code block, followed by a short bullet list of the changes made and why.

## Success Criteria
- [ ] Output is identical to the original for the same inputs.
- [ ] Measurable or clearly reasoned performance improvement.
- [ ] All variable names are descriptive and PEP 8-compliant.
- [ ] Non-obvious logic is commented.

## Validation & Reporting
- Before finishing, confirm behavior is unchanged.
- Report: list of optimizations, list of renamed variables, any new dependency.
```

**Notes:** Inferred Python version is unconstrained — confirm if a specific version matters. Assumed a single script; adjust Scope if there are several.

---

## Principles (why the structure is shaped this way)

- **One framework, scalable depth.** Non-technical prompts use 5 sections; complex ones use 10. The user learns one mental model, not many.
- **Bookend the boundaries.** Scope first, Constraints early, the key constraint restated in Validation — executors drift, and repetition at the edges is the cheapest insurance.
- **Separate the *why* from the *what*.** Context carries the why so Instructions can stay terse and unambiguous.
- **Make "done" verifiable.** Success Criteria + Validation convert a subjective request into a closed loop. This is the section rough prompts always lack and the one that most improves results.
- **Preserve the user's specifics exactly.** Names, values, file paths, and required wording from the rough prompt must survive verbatim into the structured version.

## Communication

The user may range from non-technical to expert. If the rough prompt is casual and the task simple, keep the explanation light and just deliver the Tier 1 structure. If they're clearly technical, you can be terse and skip hand-holding. Always deliver the structured prompt as a single copy-pasteable code block.
