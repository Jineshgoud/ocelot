# Ocelot

![Ocelot](./docs/ocelot-hero.jpg)

Self-hosted agent workspace. Web, Electron desktop and Expo mobile clients over one API,
with a worker for background runs. Bring your own model and computer provider, or run the
whole stack locally against Postgres.

In active development — not a release.

## Built on Rakazo

Ocelot is a fork of [elie222/rakazo](https://github.com/elie222/rakazo), Apache-2.0. `LICENSE`
and `NOTICE` are retained verbatim, and [`OCELOT.md`](OCELOT.md) states what this fork changes.
Ocelot is independent of the upstream authors and is not endorsed by them; the Rakazo name and
marks belong to them. Upstream's marketing site (`apps/www`, which served rakazo.com) and its hero image were removed
in this fork; upstream's own README is preserved in git history at the fork point.

## Monorepo

**Apps**

| App | What it is |
|---|---|
| `apps/api` | API + background worker entry points, Prisma over `pg`, ORPC routes |
| `apps/web` | Vite web client — the main surface |
| `apps/desktop` | Electron shell with `electron-updater` and Sentry |
| `apps/mobile` | Expo / expo-router client |
| `apps/worker` | Long-running job executor |
| `apps/capabilities-showcase`, `apps/www` | Internal showcase and marketing surface |

**Packages**

| Package | Role |
|---|---|
| `packages/adapters` | Model and computer providers — sandbox SDKs (Boxd, Boat, Aliyun ECS/AppStream, E2B), S3/KMS, chat adapters (Slack, Telegram, WhatsApp), Composio |
| `packages/adapter-kit` | Provider interface shared by the above |
| `packages/core` | Threads, runs, agent loop |
| `packages/db` | Prisma client + migrations |
| `packages/contracts` | Shared schemas/types |
| `packages/auth` | Sessions and auth |
| `packages/chat-ui`, `packages/ui-web`, `packages/ui-tokens` | UI layer |
| `packages/memory`, `packages/logging`, `packages/testkit` | Runtime support |

## Status: rebrand in progress

Ocelot branding has landed in the welcome, auth and onboarding flows and in the provider
picker (`apps/web/src/components/ProviderMark.tsx`, `apps/mobile/lib/provider-mode.ts`).
Around **116 files still use `RAKAZO_`-prefixed env vars** (for example
`VITE_RAKAZO_LOCAL_EMAIL`), so expect both names to appear until the rename is finished.
Upstream's translated READMEs (`README.ja.md`, `README.zh-CN.md`, `README.th-TH.md`) are
untouched upstream text.

## Requirements

- Node `^22.22.2 || ^24.0.0 || >=26.0.0`
- pnpm `9.15.0` (declared as `packageManager` — use corepack)
- PostgreSQL

## Getting started

```bash
corepack enable
pnpm install
cp .env.example .env     # set DATABASE_URL, BETTER_AUTH_SECRET (32+ chars), BETTER_AUTH_URL
pnpm db:generate
pnpm db:migrate
pnpm dev
```

`pnpm dev` runs `scripts/dev.mjs`, which brings up the web client, API and worker together.

## Scripts

| Command | Does |
|---|---|
| `pnpm dev` | All services via `scripts/dev.mjs` |
| `pnpm build` | Turbo build across the workspace |
| `pnpm check` | Turbo type/lint checks |
| `pnpm lint` / `pnpm format` | Biome |
| `pnpm db:generate` / `pnpm db:migrate` | Prisma generate / migrate |
| `pnpm test` | Unit tests (node:test + vitest) |
| `pnpm test:integration` / `pnpm test:e2e` | Harness-driven suites |
| `pnpm sandbox:build` | Build the local computer sandbox image |

## License

Apache License 2.0 — see [`LICENSE`](LICENSE). Third-party notices are in [`NOTICE`](NOTICE)
and must be retained when redistributing.
