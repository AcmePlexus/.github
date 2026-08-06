# AcmePlexus shared CI

Reusable GitHub Actions workflows, so every site is built and checked by the same
pipeline instead of each repo carrying a copy that drifts.

Public because client-owned repositories call these too — a private repo's reusable
workflows are not readable from another organisation. **They contain no secrets.**
Everything sensitive is supplied by the calling repository, or read at run time from AWS
Parameter Store via OIDC.

| Workflow | For | What it does |
|---|---|---|
| `astro.yml` | static Astro sites | typecheck, lint, build, `wrangler deploy` |
| `app-monorepo.yml` | pnpm monorepos (Bun/Hono + Vite) | checks, build image, push to GHCR, point Coolify at the tag |
| `payload.yml` | Next + Payload + Postgres | checks against an ephemeral Postgres; Coolify still deploys |

## Use

```yaml
# .github/workflows/ci.yml in the client repo
name: CI
on:
  push: { branches: [main] }
  pull_request:
jobs:
  ci:
    uses: AcmePlexus/.github/.github/workflows/payload.yml@main
    secrets: inherit
```

Operational detail lives with the platform, not here.
