# Tools — the hands on the world

A soul with no tools is a very well-spoken person locked in a room. Tools are how
the agent *acts*: reads a file, runs a command, queries your CI, opens a PR,
searches the web. The other four pillars decide *what* to do and *why*; tools are
*how* it actually gets done.

## Two layers of tools

### 1. Harness-native tools — always there

Your agent harness ships a baseline set. In ours:

| Tool | Use |
|------|-----|
| **Read / Write / Edit** | look at and change files — *prefer these over shell `cat`/`sed`* |
| **Glob / Grep** | find files and search content — *prefer over shell `find`/`grep`* |
| **Bash** | run anything: `git`, `gh`, package managers, your own CLI |
| **Agent** | spawn focused sub-agents for parallel or multi-step work |
| **Web search / fetch** | look up docs, errors, APIs |

The soul's job here is mostly *discipline*: "use the dedicated file tools, not
shell equivalents," "never run a blocking foreground `tail -f` — it dies with the
session," "confirm before destructive Bash."

### 2. MCP servers — your platform's tools

**MCP (Model Context Protocol)** is how you give the agent tools *specific to your
world*: your CI's API, your issue tracker, your cloud docs, your own DevOps CLI.
Each MCP server exposes a set of named tools the agent can call like any other.

```
agent  ──calls──▶  mcp__teamcity__build_log     (your CI)
       ──calls──▶  mcp__atlassian__getJiraIssue  (your tracker)
       ──calls──▶  mcp__aws-docs__search         (cloud docs)
       ──calls──▶  mcp__yourcli__deploy          (your own CLI, wrapped)
```

The highest-leverage MCP you can build is usually **a thin wrapper around your
own CLI** — the same `deploy`, `db populate`, `env up` commands your engineers
already run. Now the agent runs them too, with the same safety rails.

## Wiring MCP — the mental model

1. **Register** the server (command to launch it + any auth). See
   `mcp.example.json`.
2. On startup the harness **connects** and the server's tools appear, namespaced
   `mcp__<server>__<tool>`.
3. The agent **discovers** them — often lazily: a tool's full schema is loaded
   only when it's about to be used, so hundreds of tools cost little until called.
4. The agent **calls** them like any native tool.

If a tool the user wants isn't connected, the agent should say so and point at
how to enable it — not silently pretend the capability exists.

## The safety contract — tools are where damage happens

Reading is free; *writing* is where an agent can do real harm. Bake these into
the soul, because tools execute whatever the reasoning decided:

- **Read-only is always fine** — `describe-*`, `get-*`, `list-*`, `status`,
  `plan`. Run freely.
- **Mutations get confirmation** — anything that creates, updates, deletes, tears
  down, or touches production. Print the exact command + target, then ask.
- **Prefer a dry-run first** when the tool offers one (`--dry-run`,
  `terraform plan`, `--no-execute`).
- **Mark destructive and long-running tools** in their registration so the agent
  warns before calling and never fires them speculatively.
- **Sequential work runs synchronously** — wait, check the result, then proceed.
  Don't background steps that depend on each other.

## The division of labor, one more time

> **Tools** are capability. **Soul** is restraint. A powerful toolset without a
> soul to govern it is exactly the agent everyone is nervous about. The two ship
> together or not at all.

See `mcp.example.json` for a registration example.
