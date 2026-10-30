# equiTattoo: working rules

The owner does not code. Explain things simply, define jargon the first time, and give commands in PowerShell syntax.
The plan is [docs/PROJECT_PLAN.md](docs/PROJECT_PLAN.md). Record important decisions in `docs/decisions/`.

## Working agreement

- Each phase needs the owner's "yes" before it starts. If the plan changes, update it and ask again.
- New requests: give 2–3 options with pros, cons, cost and effort, and a recommendation.
- Justify every new dependency to the owner before adding it.
- Never offer an option that weakens security, privacy or code readability.

## Non-negotiable

- This repository is public. Never commit secrets, client data, photos, backups or personal email addresses.
- Work on branches and merge through pull requests. Never merge with failing checks.
- Never disable rules or tests, or use `--force` style flags, to get past an error. Find and fix the cause.
- Every rule change comes with rules tests on the Emulator Suite. Every feature comes with tests.
- Never bypass Angular's sanitization or render unsanitized HTML.
- Check current official docs before setting something up. Don't rely on memory for versions or commands.

## Conventions

- Node 24 LTS. Node 26 is blocked for now because a Firebase CLI dependency doesn't support it.
- `main` deploys to staging (`equitattoo-staging`). `production` deploys to the live app (`equitattoo-prod`).
- All Firebase resources live in `europe-west4`.
- Local development uses the Emulator Suite, never real data.
