# Memory — what survives the session

Context tells the agent what is true *today*. Memory tells it what it learned
*before today* and should not have to be taught again. Without memory, every
session starts from zero: the agent re-asks your preferences, re-discovers that
you like draft PRs, re-learns the workaround you explained last week. Memory is
what turns a capable stranger into someone who *remembers you*.

## The shape: one fact, one file

Memory here is not a database and not a vector store — it's a **folder of small
markdown files**, one fact per file, plus an always-loaded index. Plain files,
because they're inspectable, editable, diffable, and you can delete a wrong one
by hand.

```
memory/
├── MEMORY.md                  ← the index: one line per memory, ALWAYS loaded
├── prefers-draft-prs.md       ← one fact
├── deploy-window-fridays.md   ← one fact
└── …
```

### Each memory file

```markdown
---
name: prefers-draft-prs
description: <one line — used to decide relevance during recall>
metadata:
  type: user | feedback | project | reference
---

<the fact itself. For feedback/project, follow with **Why:** and
**How to apply:** lines. Link related memories with [[their-name]].>
```

### The index — `MEMORY.md`

The index holds **one line per memory** and is loaded into context every session:

```markdown
- [Prefers draft PRs](prefers-draft-prs.md) — always open PRs as drafts
```

Memory content never goes in the index — only the pointer. The index is the
agent's table of contents; the files are the chapters, pulled in by relevance.

## The four types

| Type | What it captures | Example |
|------|------------------|---------|
| **user** | who the user is — role, expertise, preferences | "prefers TypeScript strict mode everywhere" |
| **feedback** | how the agent should work — corrections & confirmed approaches, *with the why* | "don't add comments unless asked — Why: clutters diffs" |
| **project** | ongoing goals/constraints not derivable from code or git | "migrating sale-pages to Vue 3 through Q3" |
| **reference** | pointers to external resources | "metrics dashboard: https://…" |

## How recall works

1. The **index** rides in context every session — so the agent always knows
   *what it knows about*, even if not the detail.
2. When a request touches a remembered topic, the matching file is **pulled in by
   relevance** (its `description` is the matching key).
3. The agent applies the fact — but treats it as *what was true when written*. If
   a memory names a file or flag, it **re-verifies that it still exists** before
   relying on it. (Memory is recall, not gospel — see the Invariants in the soul.)

## What to save — and what not to

**Save** what's durable and non-obvious: a preference, a hard-won correction, a
project constraint the code doesn't reveal.

**Don't save** what the repo already records — code structure, past fixes, git
history, things in your CONTRIBUTING.md. If asked to "remember" one of those, ask
what was *non-obvious* about it and save *that*.

**Before saving**, check the folder for a file that already covers it — update
that one rather than spawning a duplicate. **Delete** memories that turn out
wrong; a confidently-recalled stale fact is worse than no memory at all.

See `prefers-draft-prs.example.md` for a complete memory file.
