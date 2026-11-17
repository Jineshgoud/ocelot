# Ocelot

![Ocelot](./docs/ocelot-hero.jpg)

### Agents that run on your metal.

Most agent products are a chat box you rent from someone else: their model, their computer,
their memory, their bill. Ocelot is the opposite shape — the entire stack, in a repo you can
run. Your model keys. Your sandboxes. Your database. Your phone.

Bring your own model, bring your own computer, or run the whole thing offline on a laptop
with Postgres in Docker. Nothing about the product assumes you are someone else's tenant.

---

## Windows into the same brain

One API, four surfaces, no feature forks:

| Surface | What it gives you |
|---|---|
| **Web** (`apps/web`) | The full workspace — threads, runs, approvals, files |
| **Desktop** (`apps/desktop`) | Electron, auto-updating, native shell around the same app |
| **Phone** (`apps/mobile`) | Expo client for iOS and Android |
| **Chat** (`packages/adapters`) | Slack, Telegram, WhatsApp and iMessage, as first-class transport |

Your agent doesn't live in a browser tab. It answers in Slack, keeps working in the
background while your laptop is shut, and shows the same thread wherever you open it.

## What makes it different

**The computer is pluggable, not implied.** Agents need somewhere to run. Ocelot treats that
as an interface: Boxd, Boat, E2B desktops, Aliyun ECS/AppStream, or a local Docker sandbox.
Swap the provider, keep the agent. Nothing is welded to one vendor's VM.

**Approval is architecture, not a checkbox.** Every consequential action can route through
`action-approval` — there's an effect-key system so a retried action can't be double-applied,
agent verification, and an explicit AI-consent layer with an audit trail. Content can be
encrypted with your own KMS keys. Read [`docs/approvals.md`](docs/approvals.md) and
[`docs/agent-verification.md`](docs/agent-verification.md) — the thinking is documented, not
buried.

**It is actually operable.** Not "works on my machine" — there are forty-odd operator docs
covering production deploys, backups, observability, secret managers (Infisical), fleet
management, restricted networks, and upgrading. That's the difference between a demo and
something you can leave running on a Tuesday night.

## How it hangs together

```
    phone          desktop           web            Slack / Telegram / WhatsApp / iMessage
      │               │               │                            │
      └───────────────┴───────┬───────┴────────────────────────────┘
                              │
                     ┌────────▼─────────┐
                     │     apps/api     │  ORPC endpoints
                     │  apps/worker     │  background runs
                     └────────┬─────────┘
                              │
        ┌─────────────┬───────┴────────┬──────────────┐
   packages/core  packages/adapters  packages/db   packages/memory
   threads, runs  models, computers,  Prisma + pg   recall over time
   approvals      chat, integrations
```

## Sixty seconds to running

Needs Node `^22.22.2 || ^24.0.0 || >=26.0.0` and pnpm `9.15.0` (via corepack), plus Postgres.

```bash
corepack enable
pnpm install
cp .env.example .env      # DATABASE_URL, BETTER_AUTH_SECRET (32+ chars), BETTER_AUTH_URL
pnpm db:generate
pnpm db:migrate
pnpm dev                  # web + api + worker, together
```

`pnpm dev` runs `scripts/dev.mjs`, which starts the web client, API and worker in one go.

## Bring your own everything

| Layer | Options |
|---|---|
| **Models** | Any provider you configure — keys stay yours (`docs/self-host-models.md`) |
| **Computers** | Boxd · Boat · E2B · Aliyun ECS / AppStream · local Docker |
| **Chat** | Slack · Telegram · WhatsApp · iMessage |
| **Integration** | Composio, plus a plugin system (`docs/build-a-plugin.md`) |
| **Secrets** | `.env`, or Infisical (`docs/infisical-secrets.md`) |

## Beyond the chat loop

`docs/` reads like the product itself: **memory** that persists across sessions, **routines**
for scheduled work, **skills** and **plugins** to extend the agent, **spaces** and **groups**
for multi-agent and multi-human setups, **voice and calls**, **fleet management** for running
many agents, and **import-an-agent** for bringing one over from elsewhere.

## Scripts

| Command | Does |
|---|---|
| `pnpm dev` | Web + API + worker via `scripts/dev.mjs` |
| `pnpm build` | Turbo build across the workspace |
| `pnpm check` | Type and lint checks |
| `pnpm test` | Unit tests (node:test + vitest) |
| `pnpm test:integration`, `pnpm test:e2e` | Harness-driven suites |
| `pnpm db:generate`, `pnpm db:migrate` | Prisma generate / migrate |
| `pnpm sandbox:build` | Build the local computer sandbox image |

## Where to read next

Start with [`docs/getting-started.md`](docs/getting-started.md), then
[`docs/computer.md`](docs/computer.md) for the sandbox model and
[`docs/self-host-production.md`](docs/self-host-production.md) when you're ready to make it
permanent.

---

Apache License 2.0 — see [`LICENSE`](LICENSE), [`NOTICE`](NOTICE) and [`OCELOT.md`](OCELOT.md).
