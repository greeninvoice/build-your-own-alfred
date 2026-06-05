# Anti-patterns — how to build a soulless agent

The fastest way to understand the five pillars is to watch each one fail. Every
anti-pattern below produces an agent that is *technically* configured and
*practically* useless — or worse, confidently wrong. If your agent feels off,
find it here first.

---

## Soul anti-patterns

### 🚫 The bloated soul
You poured every convention, recipe, and edge case into `SOUL.md` because "the
agent should always know it." Now every "good morning" drags 12,000 tokens of
deployment trivia, the model's attention is spread thin across rules it doesn't
need this turn, and the actual character is buried on page nine.

> **Fix:** the soul holds *character and law* only. Recipes go in skills. If a
> section isn't relevant to *most* turns, it's a skill, not a soul section.

### 🚫 The personality-free soul
"You are a helpful AI assistant for our platform." No character, no voice, no
refusals. The result reverts to generic-chatbot register the moment anything
gets hard, because there's nothing specific to revert *from*.

> **Fix:** pick a character with a known voice. "Alfred wouldn't grovel" encodes
> more behavior in four words than a paragraph of tone description.

### 🚫 The untiered rule list
Twenty rules, all flat, all equal. When "be concise" collides with "verify
before stating," the agent has no way to know which wins — so it picks randomly.

> **Fix:** tier them. Invariants beat defaults beat preferences. The tiering
> *is* the conflict-resolution logic.

### 🚫 The soul that decays mid-session
Great for the first three replies, generic by reply thirty. The base harness's
defaults reassert themselves whenever attention lapses.

> **Fix:** name the **counter-defaults** explicitly — the specific base behaviors
> to override every turn. A soul that doesn't fight the base prompt loses to it.

---

## Skill anti-patterns

### 🚫 The skill that never fires
Its description reads "information about deployments." Too vague — the dispatcher
can't tell when it's relevant, so it either never loads or loads for every
mention of the word "deploy."

> **Fix:** describe the *trigger action* and the *negative scope*. "Use when about
> to TRIGGER a deploy — NOT for casual CI chat." Sharp boundaries, reliable routing.

### 🚫 The mega-skill
One file covering deployment *and* testing *and* events *and* conventions.
Routing becomes a coin-flip and the agent loads 8,000 tokens to learn one fact.

> **Fix:** one skill, one job. Split until each file has a single clear trigger.

### 🚫 The skill the agent ignores
You wrote a beautiful skill, but the soul never says "don't answer this topic
from memory" — so the model answers from training data and never opens the file.

> **Fix:** the soul must *forbid* improvising on covered topics. The skill is
> only as good as the rule that forces its use.

### 🚫 The background-essay skill
Three pages of architecture philosophy before the first actual command. The
agent loaded it to *act*, not to read your manifesto.

> **Fix:** lead with the command, the schema, the gotcha. Background last, if at all.

---

## Context anti-patterns

### 🚫 The hard-coded inventory
"We have 30 backend services" baked into the prompt. The day someone adds the
31st, the agent lies — and nobody notices until it confidently denies a service
exists.

> **Fix:** generate inventory at startup by scanning the real repos. Never author
> a fact that has a source of truth you could read instead.

### 🚫 The mistaken-identity bug
The agent addresses the user by the harness account's name, or an auto-memory
email — the operator of the tooling, not the person typing.

> **Fix:** resolve identity from *your* profile and tell the soul to ignore the
> harness's guess. Wrong name shatters the "knows me" illusion instantly.

### 🚫 The stale clock
No real date injected, so "the latest build" and "what shipped this week" are
meaningless and the agent quietly reasons from its training cutoff.

> **Fix:** inject the actual current date/time every session.

---

## Memory anti-patterns

### 🚫 The junk drawer
Everything gets saved — every passing comment, every one-off, every fact the repo
already records. Recall returns noise, and the signal drowns.

> **Fix:** save only the durable and non-obvious. If the repo, git history, or
> CONTRIBUTING.md already records it, don't.

### 🚫 Memory as gospel
A memory says the config lives at `src/config.ts`. It moved three weeks ago. The
agent edits a path that no longer exists because it trusted the memory blindly.

> **Fix:** treat memory as *what was true when written*. Re-verify any path, flag,
> or identifier a memory names before acting on it. Context beats memory.

### 🚫 The duplicate pile
The same preference saved five times under five names because nobody checked for
an existing file first. Updates touch one copy; the other four go stale.

> **Fix:** before saving, search for a file that already covers it and update that.

### 🚫 The contentful index
`MEMORY.md` grows the full text of every memory, so the always-loaded index
itself becomes a bloated soul.

> **Fix:** the index holds one *pointer line* per memory. Content lives in the
> file, pulled in by relevance.

---

## Tools anti-patterns

### 🚫 Capability without restraint
The agent can `terraform destroy` and `delete-*` against prod, and nothing in the
soul says to confirm first. This is the agent everyone is rightly nervous about.

> **Fix:** read-only is free; mutations get confirmation; prefer dry-runs; mark
> destructive tools. Restraint lives in the soul, not in good intentions.

### 🚫 The phantom capability
A tool isn't connected this session, but the agent pretends it ran — fabricating
output rather than admitting the gap.

> **Fix:** an unconnected tool is a capability the agent does *not* have. It says
> so and points at how to enable it.

### 🚫 The blocking foreground watcher
`docker logs -f` or `npm run dev` in the foreground — which dies the instant the
session ends, taking the work with it.

> **Fix:** hand long-running/streaming commands to the user to run in their own
> terminal; don't trap them in the agent's session.

### 🚫 Shell where a tool exists
`cat`, `find`, `grep` via bash when dedicated Read/Glob/Grep tools exist —
slower, noisier, and worse for the user's review.

> **Fix:** prefer the dedicated tool. Reserve bash for what only bash can do.

---

## The meta-anti-pattern

> **Building tools first and soul last.** A powerful toolset governed by no soul
> is the worst of all worlds: maximum capability, minimum judgment. Always write
> the soul before you widen the hands.
