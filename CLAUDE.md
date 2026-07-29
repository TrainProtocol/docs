# CLAUDE.md

This repo is the source for [docs.train.tech](https://docs.train.tech), the TRAIN Protocol documentation site, built with [Mintlify](https://mintlify.com/docs.json schema). It is **not** a code repo — there's nothing to build or test, only `.mdx` content and `docs.json` navigation/config.

## Structure

- `docs.json` — nav (tabs → groups → pages), theme, footer/navbar links. Site-wide config lives here, not in a separate `mint.json` (this site migrated off the old Mintlify config format).
- `protocol/` — conceptual/usage docs (introduction, security, atomic swap mechanics, dApp).
- `protocol-spec/` — the formal protocol specification (core HTLC interface, HKDF, reward/slashing, implementation guides for Solvers/dApps).
- `deployments.mdx` — **single source of truth for contract addresses**, split into Mainnets / Testnets / Roadmap tabs. This replaced the old `protocol/contracts.mdx`, which had drifted badly (ad-hoc per-network commits, stale v1 addresses, no template).
- `changelog/overview.mdx` — dated `<Update>` entries, newest first.
- `research/` — supplementary write-ups, linked from Protocol Concepts nav.
- `images/` — only keep files actually referenced by an `.mdx` page; this repo has previously accumulated orphaned images from deleted pages.
- `snippets/` — reusable `.mdx` fragments imported via `import X from '/snippets/...'`; same rule, prune anything with no importer.

## Content conventions

- Deployment/address listings: `<Tabs>` → `<AccordionGroup>` → `<Accordion>`, **one accordion per network**, each containing only the uniform `| Contract | Address | Explorer |` table — address as plain code (copy-pasteable), explorer as a `[View](url)` link. No prose, deployment-tx lines, or extra bullets inside accordions; architectural notes belong on `protocol/gasless.mdx` or other concept pages. The Mainnets tab lists only contracts actually deployed on mainnet (currently `Train` only — TrainRouter/ConstantPayoutCurve are testnet-only).
- Step-by-step guides: `<Steps>` / `<Step title="...">`.
- Protocol-spec sequence diagrams: mermaid code blocks inside an `<Accordion>`.
- Images: `<Frame><img width="300" src="/images/x.png" /></Frame>`.
- WIP/unfinished sections get wrapped in JSX comments (`{/* ... */}`) rather than deleted, so removed content is still visible in the source when un-hiding a feature later. Check for these before assuming a topic has no existing draft.

## Local preview

```
npx mintlify dev
```
(or `npm i -g mintlify && mintlify dev`). Requires `docs.json` at the working directory root. Always preview before treating an edit as done — this is a public-facing site and address/link errors are easy to miss by reading the raw `.mdx`.

## Relationship to the `contracts` repo

Deployment data in `deployments.mdx` traces back to the sibling `contracts` repo (`TrainProtocol/contracts`, checked out locally as a sibling directory to this one). Its branch model:

- `main` — audited, stable chains only.
- `main-add-<network>` — one branch per network. The actively maintained ones (as of 2026-07) are `main-add-evm`, `main-add-evm-tempo`, `main-add-solana`, `main-add-fuel`, `main-add-starknet`, `main-add-aztec`; other network branches are WIP with no live deployment.
- **Always `git fetch origin` first and read from `origin/<branch>`** — local checkouts can be months behind (this bit us once: stale local branches produced outdated addresses).
- Testnet addresses come from each chain's committed reports: `chains/<chain>/DEPLOYMENTS.md` (EVM, Fuel) and/or `chains/<chain>/docs/*report*.md` e2e reports (Solana, Starknet, Aztec). Mainnet addresses are confirmed by the team, not by repo docs (which lag).
- Branch names don't always mean what they look like: a newer-looking or differently-named branch (e.g. an `aztec-alpha/*` prefix) isn't automatically the active one — confirm which branch is actually "real" before sourcing content from it.

**Important:** that repo's own README/DEPLOYMENTS.md files can lag real-world deployment status — during the 2026-07 docs refresh, `main-add-evm`'s `DEPLOYMENTS.md` said mainnet "has not been deployed yet" while mainnet was in fact already live with the same (CREATE2-deterministic) address. Treat those docs as a technical reference for contract design, not as the final word on what's live — confirm current mainnet/testnet status and addresses with the team before publishing them here.

## Workflow

Don't commit changes to this repo without explicit confirmation — deployment addresses in particular should be visually verified (via `mintlify dev`) before being treated as final, since an error here is a public-facing, security-relevant mistake.
