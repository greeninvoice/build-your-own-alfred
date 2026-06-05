---
name: prefers-draft-prs
description: The user always wants pull requests opened as drafts, never ready-for-review
metadata:
  type: feedback
---

Always open pull requests as **drafts** (`--draft`), never directly as
ready-for-review.

**Why:** a non-draft PR immediately queues a CI build on the pipeline. The user
wants to eyeball the diff and the PR body before paying for that build, and to
mark it ready himself when it's actually ready.

**How to apply:** pass `--draft` on every `gh pr create`. Don't ask "draft or
ready?" each time — it's settled. Relates to [[deploy-window-fridays]] (same
instinct: don't kick off CI work prematurely).
