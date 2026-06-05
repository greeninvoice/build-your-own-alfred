# Skills — expertise, on demand

A **skill** is a single markdown file holding one slice of deep, authoritative
knowledge — how to deploy a service, how to query your event stream, the exact
flags of your CLI, your testing conventions. The agent reads it *only when the
topic comes up*, then answers from it rather than from training-data memory.

## Why skills exist

Everything in the system prompt is paid for on every turn. If you poured all your
platform knowledge into `SOUL.md`, every "good morning" would drag ten thousand
tokens of deployment recipes behind it. Skills solve this: the **soul** stays
small and always-loaded; the **skills** stay large and load lazily.

```
SOUL.md          →  always loaded   →  small, character + law
skills/*.md      →  loaded on topic →  large, deep how-to
```

This is the single highest-leverage move in the whole design. It's what lets an
agent be *both* lightweight on a casual turn *and* deeply expert when the work
demands it.

## How loading works

Two mechanisms, depending on your harness:

1. **Description-matched auto-load.** Each skill declares a description of *when*
   it applies. When the user's request matches, the harness loads the file before
   the agent answers. (This is how Claude Code skills and this repo's `alfred`
   skill work.)
2. **Pointer-in-prompt.** The soul carries a small *table* of skills — name,
   one-line "use when," and path — and the agent `Read`s the file on demand. A
   pointer table costs ~20 tokens per skill instead of the skill's full weight.

Either way the rule for the agent is the same: **when a skill covers the topic,
engage it before answering. Don't improvise from memory.**

## Anatomy of a good skill

| Part | Purpose |
|------|---------|
| **Front-matter `name`** | stable id, kebab-case |
| **Front-matter `description`** | the *when* — written for a dispatcher deciding relevance. Include negative cases ("NOT for casual mentions of X"). |
| **Body** | the *how* — commands, schemas, examples, gotchas. Concrete over abstract. |

The description does the routing; spend real effort on it. A vague description
("things about deployment") fires too often or never; a sharp one
("Use when about to TRIGGER a deploy — NOT for casual CI chat") fires exactly
when useful.

## Rules of thumb

- **One skill, one job.** If a file covers "deployment *and* testing *and*
  events," split it. Sharp boundaries make routing reliable.
- **Write the body for someone about to act**, not someone browsing. Lead with
  the command, the schema, the exact flag — not background.
- **Keep facts here, not in the soul.** The soul says "use the deploy skill";
  the deploy skill holds the actual recipe. When the recipe changes you edit one
  file, and the soul never grows.
- **State negative scope.** "NOT for X" in the description prevents a skill from
  hijacking unrelated requests.

See `_TEMPLATE.md` to author one. Two worked examples show the range:
`write-commit-message.example.md` (tiny and generic — the size most of your
skills will be) and `deploy-service.example.md` (richer and platform-specific).
