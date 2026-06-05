---
name: write-commit-message
description: >
  Use when about to WRITE a git commit message — the user says "commit this",
  "write a commit message", or you're committing on their behalf. NOT for casual
  mentions of commits, questions about git history, or PR descriptions.
metadata:
  domain: git
---

# Write a commit message

A deliberately tiny skill — the kind you'll write most. It exists so the agent
formats commits *one consistent way* instead of improvising a new style each time.

## The shape

Use [Conventional Commits](https://www.conventionalcommits.org): a typed summary
line, and a body only when it adds something the summary can't.

```
<type>(<optional scope>): <imperative summary, ≤72 chars>

<optional body — what changed and why, wrapped at 72 columns>
```

## Types

- `feat` — a new capability
- `fix` — a bug fix
- `refactor` — a behaviour-preserving change
- `docs` / `test` / `chore` — as named

## Examples

```
feat(auth): add password-reset endpoint
fix(parser): handle empty input without throwing
```

## Don't

- Don't end the summary line with a period.
- Don't write "update code" or "fixes" — say *what* changed and *why*.
- Don't pad a one-line change with a three-paragraph body.

> This is the floor of skill-writing: one clear trigger in the description, a
> recipe, two examples, and the mistakes to avoid. Most of your skills will be
> about this size.
