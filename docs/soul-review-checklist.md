# The Soul Review Checklist

> A tear-off page. Print it, hold it against your own `SOUL.md`, and fix every box
> you can't tick. If you do nothing else after the talk, do this.

---

## Identity & voice

- [ ] The agent is introduced as a **role with reach**, not "a tool" or "an assistant."
- [ ] It is modeled on a **named character** (or a voice specific enough to be one).
- [ ] The voice is pinned with **DON'Ts** ("never sycophantic"), not just adjectives.
- [ ] There is a clear rule for **how to address the user** — and an instruction to
      **ask once** rather than guess when it's ambiguous.
- [ ] The relationship is defined: does it **defer, or push back**? When?

## Size & altitude

- [ ] The soul is **roughly two pages.** If it's longer, something is a skill in disguise.
- [ ] No deployment recipes, CLI flag tables, or domain trivia live here — those are **skills**.
- [ ] Every section earns its place on a *typical* turn. Rare-use content is elsewhere.

## Priorities

- [ ] There is a short, loud **Operating Priorities** block (3–4 items) near the top.
- [ ] "**Verify before stating**" is one of them, with authoritative sources named per domain.
- [ ] "**Use skills, don't answer from memory**" is stated explicitly.

## Rules & tiering

- [ ] Rules are **tiered** (invariants → defaults → workflow-safety), not a flat list.
- [ ] Each **invariant** is a real failure if broken — not a style preference.
- [ ] There's an **attribution/provenance** rule (commits/PRs name the agent + real model).
- [ ] There's a **safety rule for irreversible actions** (drafts, prod confirmation, no force-push).
- [ ] There's a **scope boundary** (which repos/systems the agent may touch).

## Counter-defaults

- [ ] The soul names the **specific base-harness behaviors** it must override every turn.
- [ ] At minimum: verify-after → verify-first; speculate-on-unopened-files → read-first;
      skip-skills → load-skills; generic-voice → *your* voice.

## Pillar hooks

- [ ] The soul knows **context** is injected and is **authoritative over memory**.
- [ ] The soul knows **memory** exists, where it lives, and to **re-verify** what it recalls.
- [ ] The soul carries the **tools safety contract** (read-only free, mutations confirmed).

## The read-aloud test

- [ ] Read the whole soul **out loud.** Does it sound like **a person**, or a policy PDF?
- [ ] Could a colleague who knows the character **predict** how the agent will reply? 
- [ ] Is there **one cardinal sin** the agent will never commit? Name it: ____________

---

### Scoring (be honest)

- **18–24 ticked** — you have a soul. Ship it and let memory accumulate.
- **10–17** — a draft with a pulse. Tier the rules and add counter-defaults next.
- **under 10** — you have a config file, not a soul. Start with identity and voice.
