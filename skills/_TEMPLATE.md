---
name: your-skill-name
description: >
  Use when [the precise trigger — what the user is ABOUT TO DO]. Mention the
  concrete verbs and nouns that should fire it ('deploy X', 'what's the latest
  build of Y'). Then state the NEGATIVE scope: NOT for [the lookalike requests
  that should NOT fire it — casual mentions, adjacent topics handled elsewhere].
metadata:
  domain: [e.g. ci / events / testing / aws]
---

# [Skill Title]

<!-- One sentence: what this skill lets the agent do, and when it matters. -->

## When you're here, you're about to [verb]

<!-- Orient the agent: it loaded this because the user wants to do X. Lead with
     the most common action, not with background theory. -->

## The recipe

<!-- The actual how-to. Be concrete. Real commands, real flags, real paths.
     If there's a single canonical command, put it first and unmissable: -->

```bash
# The command that does the thing
your-cli do-the-thing --flag value
```

## Inputs / schema

<!-- If the agent must construct a payload, query, or structured call, give the
     exact shape. A filled example beats a description of the fields. -->

## Gotchas

<!-- The things that bite. The flag that's easy to forget, the prod-vs-local
     distinction, the step that must come first. This section is often the most
     valuable part of the whole skill — it's the institutional knowledge that
     isn't in any README. -->

- [Gotcha 1]
- [Gotcha 2]

## Don't

<!-- Explicit anti-patterns. What looks right but isn't. -->

- Don't [common mistake].
