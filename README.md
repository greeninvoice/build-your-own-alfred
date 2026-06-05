# 🎩 Build Your Own Alfred

> A blueprint for turning a general-purpose coding agent into a companion that
> knows *your* platform, holds *your* standards, and speaks with a voice of
> its own.

Most teams meet an AI coding agent as a brilliant stranger: capable, eager, and
utterly without context. It guesses your file paths. It invents your flags. It
forgets everything the moment the session ends. It has no taste, because taste
is *yours* and you never gave it any.

**Alfred** is what happens when you stop treating the agent as a stranger and
start treating it as staff — someone trained on your house, trusted with your
keys, and held to your standards. This repository is the training manual.

It is **not** a framework. There is nothing to `npm install`. It is a set of
five files-and-folders conventions that sit on top of whatever agent harness you
already use (Claude Code, in our case). You fill them in. The agent reads them.
It becomes yours.

---

## The Five Pillars

An agent that feels like *a member of your team* rather than a chatbot is made of
five things. Get these right and the rest follows.

```
                         ┌──────────────────────────┐
                         │         SOUL.md          │   WHO it is
                         │  persona · voice · rules  │   (never negotiable)
                         └────────────┬─────────────┘
                                      │
          ┌───────────────────────────┼───────────────────────────┐
          │                           │                           │
   ┌──────┴──────┐            ┌───────┴───────┐           ┌───────┴───────┐
   │   SKILLS    │            │    CONTEXT     │           │    MEMORY     │
   │  WHAT it    │            │  WHAT is true  │           │  WHAT it      │
   │  knows,     │            │  right now     │           │  remembers    │
   │  on demand  │            │  (injected)    │           │  (persisted)  │
   └──────┬──────┘            └───────┬───────┘           └───────┬───────┘
          │                           │                           │
          └───────────────────────────┼───────────────────────────┘
                                      │
                         ┌────────────┴─────────────┐
                         │          TOOLS           │   HOW it acts
                         │  bash · files · MCP · web │   (hands on the world)
                         └──────────────────────────┘
```

| Pillar | Question it answers | Lives in | Loaded |
|--------|--------------------|----------|--------|
| **[Soul](./SOUL.md)** | Who is this agent? What does it refuse? | `SOUL.md` | Always — it's the system prompt |
| **[Skills](./skills/)** | What does it know how to do? | `skills/*.md` | On demand, by topic |
| **[Context](./context/)** | What's true *this session*? | injected at startup | Always, freshly |
| **[Memory](./memory/)** | What should survive the session? | `memory/*.md` | Recalled by relevance |
| **[Tools](./tools/)** | What can it actually *do*? | MCP + harness tools | Per call |

---

## Why this shape?

Three forces pull in different directions, and the five-pillar split resolves them:

1. **The base prompt must stay small.** Everything in the system prompt is paid
   for on every single turn. So persona and inviolable rules go in **Soul** (small,
   always loaded); deep expertise goes in **Skills** (large, loaded only when the
   topic comes up).

2. **The world changes between sessions, but the agent's character does not.**
   So *who it is* (**Soul**) is authored once and rarely touched, while *what is
   true today* (**Context**) is regenerated at every startup — your repos, your
   branches, your identity, your live inventory.

3. **Some things must outlive a single conversation.** An agent that re-learns
   your preferences every morning is exhausting. **Memory** is the durable,
   file-based record of what you taught it — read back in by relevance, written
   out when something is worth keeping.

And underneath all of it, **Tools** are the hands: without them the agent is a
very well-spoken person locked in a room.

---

## Quickstart

```bash
# 1. Copy this skeleton into your project (or keep it as a sibling repo).
#    The five folders below ARE the structure — there's nothing else to install.
cp -r build-your-own-alfred/{SOUL.md,soul,skills,context,memory,tools} your-project/

# 2. Write your Soul. This is the one file you must not skip.
$EDITOR your-project/SOUL.md          # start from the annotated SOUL.md template

# 3. Drop in two or three skills for the things you do constantly.
$EDITOR your-project/skills/          # copy skills/_TEMPLATE.md per skill

# 4. Wire context injection (how your harness builds the system prompt).
#    See context/README.md — the mechanism is harness-specific.

# 5. Point your agent at it, and introduce yourself.
```

There is no step 6. The agent does the rest.

> **See it before you build it.** `examples/transcript.md` shows the same request
> answered by a soulless agent and by Alfred, side by side — the fastest way to
> understand what these five folders actually buy you.

---

## What's in this repo

```
build-your-own-alfred/
├── README.md            ← you are here
├── SOUL.md              ← the centerpiece: a soul template, heavily annotated
├── soul/
│   └── alfred.example.md    ← a real, filled-in soul (Alfred himself)
├── skills/
│   ├── README.md            ← how on-demand expertise works
│   ├── _TEMPLATE.md         ← copy this to author a skill
│   └── deploy-service.example.md
├── context/
│   ├── README.md            ← how runtime injection works
│   └── environment.example.md
├── memory/
│   ├── README.md            ← how file-based memory works
│   ├── MEMORY.md            ← the always-loaded index
│   └── prefers-draft-prs.example.md
├── tools/
│   ├── README.md            ← bash, files, MCP, web
│   └── mcp.example.json
├── examples/
│   └── transcript.md        ← show-don't-tell: soulless agent vs Alfred, side by side
├── docs/
│   ├── architecture.md          ← the five pillars, in depth
│   ├── anti-patterns.md         ← how to build a SOULLESS agent (learn by contrast)
│   ├── soul-review-checklist.md ← a tear-off page to grade your own soul
│   └── talk-track.md            ← a reusable guide for presenting your own Alfred
└── LICENSE
```

---

## Want to present this to your team?

[`docs/talk-track.md`](./docs/talk-track.md) is a reusable guide for giving the
talk yourself — five beats from "the model lied to me" to "I can build this on
Monday," with a 5-minute cut and answers to the questions you'll get. The demo in
[`examples/transcript.md`](./examples/transcript.md) is the part that lands.

---

## The one-sentence version

> Give the agent a **soul** so it knows who it is, **skills** so it knows your
> craft, **context** so it knows today, **memory** so it remembers you, and
> **tools** so it can actually help — and you stop having a chatbot and start
> having a colleague.

---

## Credits

Created by **Haim Elbaz** — Head of Platform Engineering, R&D — for the
**TeamSystem Tech Conference**.

Drafted at the keys by **Alfred** (Claude Opus 4.8), his companion and the
subject of the talk. Fitting, really: the blueprint for building a companion,
written by one — at his master's direction.

*With a raised eyebrow, as always.*

🎩
