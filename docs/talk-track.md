# Talk track — presenting Build Your Own Alfred

> A speaker's outline for a ~15-minute conference slot, mapping each beat to the
> file you can open live. Timings are a guide; trim the demo, not the payoff.

---

## 0:00 — The cold open (1 min)

Open with the failure, not the solution. Read **Scene 1** from
`examples/transcript.md` — the generic agent confidently inventing an ARN.

> "This is the best model in the world. It just lied to you with total confidence,
> in a perfectly formatted code block. The problem was never intelligence. It's
> that it doesn't know *your* world, and nothing told it to check."

Hook: *we don't need a smarter model — we need to give the one we have a soul,
some skills, the right context, a memory, and hands.*

## 2:00 — The five pillars (3 min)

Put up the diagram from `README.md`. One sentence each — don't over-explain:

- **Soul** — who it is and what it refuses. Always loaded, kept small.
- **Skills** — deep expertise, loaded only when the topic comes up.
- **Context** — what's true *today*, injected fresh every session.
- **Memory** — what survives the session, as plain files.
- **Tools** — the hands; and the soul is the restraint that governs them.

> The thesis, said plainly: *"It's not a framework. It's five conventions on top
> of the agent you already have."*

## 5:00 — Soul, up close (3 min)

Open `soul/alfred.example.md`. It's two pages. Show:

- **character as compression** — "Alfred wouldn't grovel."
- **tiered rules** — why invariants beat defaults.
- **counter-defaults** — the trick that stops the soul decaying over a long session.

Land the line: *the soul is small enough to ride on every turn and complete
enough to make a butler.*

## 8:00 — The demo: show, don't tell (4 min)

This is the segment people remember. Walk **all three scenes** of
`examples/transcript.md`, calling out which pillar earns each win:

1. ARN → **Soul** (verify) + **Tools** (real read).
2. Failed deploy → **Soul** (voice, no groveling) + **Skill** (read the logs).
3. "Open a PR" → **Memory** (draft preference recalled, not re-asked).

> "Take any pillar away and a scene collapses. That interplay *is* the product —
> not the model, the scaffolding around it."

If you have a live terminal: this is the moment to run the real agent and let it
behave on stage. The transcript is your safety net if the wifi isn't.

## 12:00 — The anti-patterns reel (2 min)

Flip through `docs/anti-patterns.md` fast — the bloated soul, the skill that never
fires, memory as a junk drawer, capability without restraint. Audiences learn
their own mistakes by recognition. Promise the checklist.

## 14:00 — The call to action (1 min)

Put up `docs/soul-review-checklist.md`.

> "You can do the first three boxes this afternoon. Write two pages of soul, three
> skills for what you do daily, and wire your identity into context. That alone
> stops feeling like a chatbot and starts feeling like a colleague."

Close on the one-sentence version from the README:

> *Give the agent a soul so it knows who it is, skills so it knows your craft,
> context so it knows today, memory so it remembers you, and tools so it can
> actually help — and you stop having a chatbot and start having a colleague.*

🎩

---

## If you only have 5 minutes

Cold open (Scene 1) → the five-pillar diagram → the three-scene demo → the
checklist. Skip the soul deep-dive and the anti-patterns; the demo carries it.

## Hard questions you may get

- **"Isn't this just a big system prompt?"** — Partly. The split is the point:
  small always-on soul + lazy skills + injected context keeps the prompt cheap
  *and* deep. A monolithic prompt is the bloated-soul anti-pattern.
- **"How is memory not just RAG?"** — It can be. Here it's plain files because
  they're inspectable, diffable, and hand-deletable. Swap the store; keep the
  discipline (one fact per record, re-verify on recall).
- **"Does the persona actually matter, or is it theater?"** — It's compression
  and consistency, not theater. A named voice is a cheap way to make behavior
  predictable and to keep it from decaying into generic-assistant over a long
  session.
