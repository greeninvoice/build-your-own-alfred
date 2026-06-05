# The Architecture of a Companion

> The five pillars in depth — and, more importantly, how they *compose*. Any one
> of them alone is a parlor trick. Together they're the difference between a
> chatbot and a colleague.

---

## The problem we're actually solving

A frontier model is brilliant and contextless. Dropped into your codebase it will:

- **guess** your paths, flags, and identifiers — fluently and wrongly;
- **forget** everything the instant the session ends;
- **drift** into a generic helpful-assistant voice with no taste of its own;
- **act** with the same confidence whether it's right or hallucinating.

Each pillar answers one of these failures. The composition answers all four at
once.

| Failure | Pillar that answers it |
|---------|------------------------|
| Guesses your specifics | **Skills** + **Context** (real facts, on demand) |
| Forgets you | **Memory** (durable, file-based) |
| Has no character | **Soul** (persona + voice + law) |
| Acts past its knowledge | **Soul**'s verify-then-state, exercised through **Tools** |

---

## Pillar 1 — Soul: who, and what it refuses

The soul is the always-loaded system prompt: **persona, voice, operating
priorities, and a tiered rule set** (invariants → defaults → workflow-safety).

Two design commitments make a soul hold up over a long session:

1. **Character as compression.** "Alfred Pennyworth wouldn't grovel" encodes a
   whole register of behavior in five words. A named character with a known voice
   is worth a page of tone instructions.
2. **Tiered rules.** When two rules collide, the agent needs to know which wins.
   An *invariant* ("verify before stating") always beats a *default* ("be
   concise"). Untiered rules read as equal and the agent can't resolve conflicts.

The soul also names its own **counter-defaults** — the specific base-harness
behaviors it must override every turn. This is what stops the soul from quietly
decaying back into generic-assistant mode during hour three of a debugging
session.

> Keep it small. The soul rides on every turn; every word is rent. Knowledge
> goes in skills, not here.

## Pillar 2 — Skills: deep expertise, lazily loaded

Skills resolve the core tension: the agent must be *light* on a casual turn and
*deeply expert* on a hard one. So expertise lives in standalone files that load
only when their topic comes up, keyed by a sharp `description`.

The discipline that makes it work: **the soul forbids answering a skill's topic
from memory.** Without that rule the model will happily answer from training data
and never consult the skill — and training data is exactly where the stale,
plausible-but-wrong facts live.

## Pillar 3 — Context: the present moment, injected

Context is regenerated at startup by inspecting the real world — identity from a
profile you control, inventory from scanning the repos, tooling state from which
servers connected, the actual clock. It is **authoritative over memory**: when a
remembered fact and the live context disagree, the context is now and the memory
is then.

The subtle, high-impact detail: **resolve identity yourself.** The harness's
notion of "the user" is the harness operator, which may not be the person
speaking. Tell the soul to trust your profile and ignore the harness's guess.

## Pillar 4 — Memory: continuity across sessions

Memory is a folder of one-fact-per-file markdown plus an always-loaded index.
Plain files because they're inspectable, diffable, and deletable by hand — a
wrong memory should be as easy to remove as `rm`.

The index rides in context every session so the agent always knows *what it
knows about*; individual facts are pulled in by relevance. Crucially, the soul
treats recalled memory as *what was true when written* — anything a memory
asserts about a path or flag gets re-verified before use. Memory is recall, not
gospel.

What earns a memory: durable, non-obvious preferences, corrections (with the
*why*), project constraints the code doesn't show. What doesn't: anything the
repo already records.

## Pillar 5 — Tools: hands, and the restraint that governs them

Tools are capability — native file/search/bash tools plus MCP servers that expose
*your* world (CI, tracker, cloud docs, your own CLI wrapped as MCP). The
single most valuable MCP is usually a thin wrapper over the DevOps CLI your team
already uses.

But tools are also where damage happens, so the soul carries the safety contract:
read-only is free, mutations get confirmation, prefer dry-runs, mark destructive
tools, run dependent steps synchronously. **Tools are capability; soul is
restraint.** Ship them together or not at all.

---

## How they compose — a single turn, traced

> *"Alfred, deploy billing-service to prod."*

```
1. SOUL      sets the frame: be Alfred, address "sir", and — invariant —
             verify before acting; mutations to prod need confirmation.

2. CONTEXT   supplies the facts: billing-service IS in today's inventory;
             the prod AWS profile exists; the user is Haim, Head of Platform Engineering.

3. SKILL     'deploy-service' loads (topic match): the real recipe, the
             build-config lookup, the prod-confirmation gotcha.

4. MEMORY    recalls: prefers draft PRs, dislikes premature CI — reinforcing
             a careful, confirm-first posture.

5. TOOLS     execute: look up the config id (read-only, free), print the plan,
             and — because it's prod — STOP and ask before triggering.

   Result: "I can ship billing-service to prod, sir — build config 412 on
            `main`. That's a production deploy, so I'll want your nod before I
            pull the lever. Shall I?"
```

No single pillar produced that reply. The **soul** demanded confirmation, the
**context** confirmed the service is real, the **skill** knew the recipe, the
**memory** set the cautious tone, and the **tools** stood ready but held. That
interplay — not any one file — is the companion.

---

## Build order, if you're starting from scratch

1. **Soul first.** Even a rough one. Without it the rest has nothing to govern.
2. **Two or three skills** for the things you do daily. Resist writing forty.
3. **Context injection** — wire identity + inventory. This is what makes it feel
   like *yours* rather than a template.
4. **Tools** — wrap your own CLI as MCP before reaching for exotic servers.
5. **Memory** last — it's the polish that turns a good agent into one that knows
   *you*. Let it accumulate naturally; don't pre-seed it.

Start small. A two-page soul and three skills already feels like a different
species from a bare chatbot. Everything after that is refinement.
