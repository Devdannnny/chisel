# Contributing to Chisel

Thanks for considering a contribution. Chisel is a curated open-source framework — quality and consistency matter more than throughput. Please read this before opening a PR.

## What we accept

- **New skills** that follow the PAS framework and the existing skill structure.
- **Refinements** to existing skills (clearer examples, edge cases, better defaults).
- **Improvements to PAS itself** — but the bar is high. Open an issue first to discuss the change.
- **Documentation fixes** (typos, broken links, clarity).

## What we do not accept (without prior discussion)

- Wholesale rewrites of existing skills.
- New skills that duplicate existing ones with minor variations.
- Prompts dressed up as skills with no framework or structure.
- Changes that introduce dependencies or build tooling. Chisel is intentionally plain markdown.

## Skill structure

Every skill lives in `skills/<skill-name>/` and contains at minimum:

- `SKILL.md` — frontmatter with `name` and `description`, followed by the skill body.
- (Optional) `quick-reference.md`, examples, or other supporting markdown.

The frontmatter format:

```
---
name: skill-name
description: One-line description of when this skill should be triggered.
---
```

The `name` must match the folder name. The `description` is what an agent reads to decide whether to load the skill — write it for that audience.

## PR process

1. **Open an issue first** for any non-trivial change. Describe the problem and your proposed approach. This saves both of us time.
2. Fork the repo, create a branch, make your change.
3. Open a PR against `main`. Reference the issue.
4. Keep PRs focused — one skill or one fix per PR.
5. Wait for review. Expect a response within 7 days.

## Style

- Imperative voice. ("Do X." not "You should do X.")
- No emojis unless they carry meaning.
- No marketing language inside skills. The audience is an agent, not a customer.
- Preserve user-facing specifics verbatim (file paths, exact wording, values).
- Markdown only. No build steps, no generated files.

## License

By submitting a PR, you agree that your contribution is licensed under the project's MIT license.
