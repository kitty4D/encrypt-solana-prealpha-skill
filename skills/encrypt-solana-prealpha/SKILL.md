---
name: encrypt-solana-prealpha
description: "Use when integrating Encrypt on Solana pre-alpha: #[encrypt_fn] / DSL (EUint, EVector, EBitVector, PUint), EncryptService (CreateInput, ReadCiphertext), devnet + CPI SDKs, @encrypt.xyz/pre-alpha-solana-client—or fees (ENC/SOL), EncryptDeposit, account/event/fee layouts, graph IR, access control, decryption, mock vs real FHE, tutorials/examples—or byte-level Encrypt reference lookups—or Encrypt vs ika dWallet signing."
---

# encrypt solana pre-alpha

Normative: [Encrypt Developer Guide](https://docs.encrypt.xyz/) · mdbook in [`encrypt-pre-alpha`](https://github.com/dwallet-labs/encrypt-pre-alpha) `docs/`. **Load [`references/`](references/)** for gRPC, ix, flows.

**Docs revision:** [`references/docs-revision.md`](references/docs-revision.md) — if `docs/` on `main` is past the tracked commit, **tell the user** the skill may be stale; do not silently rewrite skill files.

## pre-alpha disclaimer

- **Exploration only** — not production confidentiality.
- **No real encryption guarantee** — data can be **plaintext on-chain**; do not submit sensitive or real data.
- **Keys / trust model not final**; **devnet resets**; **no warranty**. Do not market as production FHE or private custody to end users.

## references (load on demand)

| file | load for |
| --- | --- |
| [`references/developer-guide-map.md`](references/developer-guide-map.md) | Book TOC + URLs — load before guessing |
| [`references/book-snapshots.md`](references/book-snapshots.md) | Lists all book-copy md under `references/` |
| [`references/fee-and-state-reference.md`](references/fee-and-state-reference.md) | ENC/SOL fees, seven account kinds, five event types |
| [`references/docs-revision.md`](references/docs-revision.md) | `docs/` vs `main` |
| [`references/grpc-api.md`](references/grpc-api.md) | `EncryptService`, proto, clients |
| [`references/instructions.md`](references/instructions.md) | Discriminators, ix groups |
| [`references/frameworks.md`](references/frameworks.md) | Crates, `EncryptCpi`, toolchain |
| [`references/flows.md`](references/flows.md) | Lifecycle, tests, CPI vs signer |
| [`references/dsl-types.md`](references/dsl-types.md) | `EUint*` / `EVector*` / `EBitVector*` / `PUint*` tables |
| [`references/gotchas.md`](references/gotchas.md) | Field-tested bugs, silent failures, BPF limits, CPI layout |
| [`references/performance-caveats.md`](references/performance-caveats.md) | Timing, REFHE vs TFHE, bootstrap cost unknowns |

## install & tooling

**TS:** `@encrypt.xyz/pre-alpha-solana-client` + `createEncryptClient` — [`grpc-api.md`](references/grpc-api.md). **Rust** 2024, **Solana CLI** 3.x (`build-sbf`), **Bun**, `just test-unit` / `test-examples` — [`frameworks.md`](references/frameworks.md), [`flows.md`](references/flows.md). Pin git crates per upstream `Cargo.toml`.

## environment (pre-alpha)

| resource | value |
| --- | --- |
| Encrypt gRPC (TLS) | `https://pre-alpha-dev-1.encrypt.ika-network.net:443` |
| Solana RPC | `https://api.devnet.solana.com` (typical) |
| Encrypt program id | `4ebfzWdKnrnGseuQpezXdG8yCdHqwQ1SSBHD3bWArND8` |
| source repo | `https://github.com/dwallet-labs/encrypt-pre-alpha` |

**Canonical:** program id, gRPC URL, Solana RPC, git remote — hub only; align samples.

## quick pointers

**On-chain:** first ix byte = **discriminator**; 22 user ix + `emit_event` **228** — [`instructions.md`](references/instructions.md). Common path: discs 1–4 (`create_input_ciphertext` … `execute_graph`); full metas: [instruction reference](https://docs.encrypt.xyz/).

**gRPC:** `encrypt.v1.EncryptService` — `CreateInput`, `ReadCiphertext` — [`grpc-api.md`](references/grpc-api.md).

**Model:** `#[encrypt_fn]` (scalars) or `#[encrypt_fn_graph]` (scalars + vectors) → graph → on-chain `execute_graph` / ciphertext accounts → executor + `commit_ciphertext`; decrypt via gateway ix — [`flows.md`](references/flows.md), [introduction](https://docs.encrypt.xyz/). **Field-tested gotchas** (executor bugs, silent failures, BPF limits, CPI layout): [`gotchas.md`](references/gotchas.md). **Performance** (REFHE vs TFHE, timing caveats): [`performance-caveats.md`](references/performance-caveats.md). **Book-only** (DSL incl. `EVector*` / `EBitVector*`, tutorial, PC-token/swap, fees, schemas): [`developer-guide-map.md`](references/developer-guide-map.md), [`book-snapshots.md`](references/book-snapshots.md), [`fee-and-state-reference.md`](references/fee-and-state-reference.md).

## common mistakes

| mistake | instead |
| --- | --- |
| Assuming pre-alpha ciphertexts are secret | Treat as **public / plaintext-capable** (book + repo). |
| Using `#[encrypt_fn]` with vector types | Vectors lack `HasFheTypeId` — use **`#[encrypt_fn_graph]`** from `encrypt-dsl` and invoke CPI manually. See [`gotchas.md`](references/gotchas.md). |
| Treating devnet commit times as FHE benchmarks | Pre-alpha runs **no real FHE** — all timings are mock overhead. See [`performance-caveats.md`](references/performance-caveats.md). |
| Wrong `CreateInput` **authorized** / **network_encryption_public_key** | Match **NetworkEncryptionKey** + access rules — [`grpc-api.md`](references/grpc-api.md). |
| **Encrypt** vs **ika** dWallet | ika signing / `approve_message` → **`ika-solana-prealpha`** skill, not this one. |
| Patching skill when upstream `docs/` changed | **Notify user** — [`docs-revision.md`](references/docs-revision.md). |
| Forgetting Encrypt **fees / deposits / events** | Not ika-shaped — [`fee-and-state-reference.md`](references/fee-and-state-reference.md) + [`book-snapshots.md`](references/book-snapshots.md). |

**Examples:** [encrypt-pre-alpha `chains/solana/examples`](https://github.com/dwallet-labs/encrypt-pre-alpha/tree/main/chains/solana/examples).
