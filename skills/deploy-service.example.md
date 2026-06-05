---
name: deploy-service
description: >
  Use when about to DEPLOY a backend service to an environment, or to find out
  'what's the latest deployed version of X' — anything that triggers the CI
  pipeline or queries deploy state. NOT for casual mentions of 'deploy', 'CI',
  or 'pipeline' in unrelated chat (e.g. comparing CI tools, container build
  questions, talking about someone else's deploy).
metadata:
  domain: ci
---

# Deploy a backend service

This skill covers triggering a deploy through the pipeline and reading deploy
state. It is the authoritative source — do not deploy from memory of how it
"usually" works, because the build-config IDs and branch rules change.

## When you're here, you're about to ship something

Two flavours of request land here:

- **"Deploy `billing-service` to staging."** → trigger a build.
- **"What's running in prod for `billing-service`?"** → query, don't trigger.

Always confirm which before acting. A deploy is not read-only.

## The recipe — trigger a deploy

```bash
# 1. Confirm the build config id for the service + target env
ci-cli configs --service billing-service --env staging

# 2. Trigger it on the intended branch (defaults to the service's main)
ci-cli deploy --config <BUILD_CONFIG_ID> --branch <BRANCH>
```

Print the config id, target env, and branch back to the user *before* you fire,
and wait for a go-ahead on anything touching production.

## The recipe — read deploy state (always safe)

```bash
ci-cli status --service billing-service --env prod
```

## Schema — what `deploy` returns

```json
{
  "buildId": "12345",
  "state": "queued | running | success | failure",
  "queueUrl": "https://ci.example.dev/build/12345"
}
```

Hand the user the `queueUrl` so they can watch it; don't tail the log in a
blocking foreground call — it'll be killed when the session ends.

## Gotchas

- **Prod deploys need explicit confirmation.** Staging is fine to fire on
  request; prod gets a printed plan and a "shall I proceed?" first.
- **Branch matters.** A deploy on the wrong branch ships the wrong code happily
  and silently. Echo the branch back before triggering.
- **A red build is not always your change.** Check whether `main` was already
  red before assuming the deploy broke it.

## Don't

- Don't trigger a deploy "to check if it works." Triggering *is* the action.
- Don't guess a build-config id — look it up. They are not predictable from the
  service name.
