# Presenting your own Alfred — a guide

> Built your companion and want to share it with your team? This is a reusable
> outline for a ~15-minute talk that walks an audience from "the model lied to me"
> to "I can build this on Monday." Adapt every example to *your* agent — the
> structure is what travels, not the specifics.

The talk has one job: make the room feel the difference between a chatbot and a
colleague, then hand them a path to build their own. Five beats do it.

---

## Beat 1 — Open with the failure, not the solution

Start with a request where a raw model is confidently, fluently wrong — an
invented identifier, a guessed path, a hallucinated config value. (`examples/`
shows one: a generic agent inventing an ARN in a perfectly formatted code block.)

The point to land:

> The problem was never intelligence. It's the best model in the world — and it
> still guessed, because it doesn't know *your* world and nothing told it to check.

This reframes the whole talk: *we don't need a smarter model — we need to give the
one we have a soul, some skills, the right context, a memory, and hands.*

## Beat 2 — Name the five pillars

Show the diagram from the README and give one sentence each:

- **Soul** — who it is and what it refuses. Always loaded, kept small.
- **Skills** — deep expertise, loaded only when the topic comes up.
- **Context** — what's true *today*, injected fresh every session.
- **Memory** — what survives the session, as plain files.
- **Tools** — the hands; and the soul is the restraint that governs them.

The thesis, said plainly: *it's not a framework — it's five conventions on top of
the agent you already have.*

## Beat 3 — Soul, up close

Open a real, filled-in soul (`soul/alfred.example.md`) and show it's about two
pages. Pull out the three ideas that make a soul hold up:

- **character as compression** — a named voice encodes behavior in a few words.
- **tiered rules** — invariants beat defaults, so conflicts resolve themselves.
- **counter-defaults** — naming the base behaviors to override is what stops a
  soul decaying into generic-assistant over a long session.

The line to land: *small enough to ride on every turn, complete enough to make a
character.*

## Beat 4 — The demo: show, don't tell

This is the heart of the talk. Walk the three scenes in `examples/transcript.md` —
the soulless reply beside the Alfred reply — and call out *which pillar* earns each
difference:

1. The invented identifier → **Soul** (verify-first) + **Tools** (a real read).
2. The failed deploy → **Soul** (voice, no groveling) + **Skill** (read the logs).
3. "Open a PR" → **Memory** (the draft preference recalled, not re-asked).

The takeaway:

> Take any pillar away and a scene collapses. That interplay *is* the product —
> not the model, the scaffolding around it.

A live terminal makes this land hardest — let the real agent behave in front of
the room. The written transcript stands in if a live demo isn't practical.

## Beat 5 — Anti-patterns, then a call to action

Flip quickly through `docs/anti-patterns.md` — the bloated soul, the skill that
never fires, memory as a junk drawer, capability without restraint. People learn
their own mistakes by recognition.

Then close on something they can *do*. Put up `docs/soul-review-checklist.md`:

> Write two pages of soul, three skills for what you do daily, and wire your
> identity into context. That alone stops feeling like a chatbot and starts
> feeling like a colleague.

And end on the one-sentence version from the README:

> *Give the agent a soul so it knows who it is, skills so it knows your craft,
> context so it knows today, memory so it remembers you, and tools so it can
> actually help — and you stop having a chatbot and start having a colleague.*

🎩

---

## The short version (≈5 minutes)

The failure (Beat 1) → the five-pillar diagram (Beat 2) → the three-scene demo
(Beat 4) → the checklist. The demo carries it; skip the soul deep-dive and the
anti-patterns if time is tight.

## Questions you may get

- **"Isn't this just a big system prompt?"** — Partly, and the split is the point:
  a small always-on soul + lazy-loaded skills + injected context keeps the prompt
  cheap *and* deep. A monolithic prompt is the bloated-soul anti-pattern.
- **"How is memory not just RAG?"** — It can be. Here it's plain files because
  they're inspectable, diffable, and hand-deletable. Swap the store if you like;
  keep the discipline — one fact per record, re-verify on recall.
- **"Does the persona actually matter, or is it theater?"** — Compression and
  consistency, not theater. A named voice is a cheap way to make behavior
  predictable and to keep it from decaying into generic-assistant over a long
  session.
