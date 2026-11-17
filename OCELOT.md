# Ocelot — fork notice and statement of changes

This repository is a fork of **[elie222/rakazo](https://github.com/elie222/rakazo)**,
licensed under the **Apache License, Version 2.0**.

Ocelot is an independent derivative work. It is **not affiliated with, endorsed by, or
sponsored by** the upstream authors or contributors. The name "Ocelot" is ours; the
"Rakazo" name and marks belong to them and are used here only to identify the origin of
this code, as Apache-2.0 §6 permits. Nothing in this fork should be read as a claim to
their trademarks.

## What is retained from upstream

Per Apache-2.0 §4:

- `LICENSE` — the full Apache License 2.0 text, unmodified. Do not delete.
- `NOTICE` — the upstream notice file, **retained verbatim**. It records third-party code
  the project carries:

  | Component | Origin | License |
  |---|---|---|
  | Grok Bot import adapter (`packages/core/src/agent-import/grok-bot.ts`) | [OpenMausBot](https://github.com/milind-soni/OpenMausBot), Copyright 2026 Milind Soni and OpenMausBot contributors | Apache-2.0 |
  | …which adapts the agent-importers plugin | [hermes-official-plugins](https://github.com/NousResearch/hermes-official-plugins), Copyright (c) 2026 Nous Research | MIT |

If you redistribute this, keep both files. Apache-2.0 §4(d) requires retaining the NOTICE
contents, and the MIT clause embedded in it requires retaining that copyright and
permission notice too.

## Statement of changes (Apache-2.0 §4(b))

This fork modifies the upstream software. Changes made in this fork, as of the initial
fork point, are recorded in [`docs/project/CURRENT_STATE.md`](docs/project/CURRENT_STATE.md)
in the `ocelot-workspace` repo and summarise as roughly:

1. **Model resolution no longer prefers the CLI host model**
   (`packages/adapters/src/run-model.ts`, `executor.ts`). The configured connection is
   consulted first; the CLI host model is demoted to a last-resort runtime fallback.
2. **Optimistic send.** The user's own message is painted on accept rather than after the
   multi-query remote transaction resolves, and is rolled back on failure.
3. **Thinking state surfaced.** `thread.meta` is reduced into the thread snapshot so the
   working row can show Working… / Thinking… with elapsed time. Raw reasoning text is
   deliberately not placed in public events.

This list is not exhaustive and will drift as the fork advances. For an authoritative diff:

```bash
git remote add upstream https://github.com/elie222/rakazo.git   # if absent
git fetch upstream
git diff upstream/main...HEAD
```

## Fork currency

The local clone this fork was created from was at upstream commit `75712f7` (2026-11-01),
while upstream `main` had reached `3198c3e` (2026-11-17) — 16 days behind at fork time.
`NOTICE` did not yet exist locally because upstream added it after that point. Merging
upstream before publishing anything is advisable, both for the notice and for fixes.
