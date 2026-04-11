---
name: encrypt-solana-prealpha
description: "Use when integrating Encrypt on Solana pre-alpha: #[encrypt_fn] / EUint graphs, EncryptService (CreateInput, ReadCiphertext), devnet program + CPI (encrypt-pinocchio, encrypt-native, encrypt-anchor), @encrypt.xyz/pre-alpha-solana-client—or choosing Encrypt vs ika dWallet signing."
---

# encrypt solana pre-alpha

Normative: [Encrypt Developer Guide](https://docs.encrypt.xyz/) · mdbook in [`encrypt-pre-alpha`](https://github.com/dwallet-labs/encrypt-pre-alpha) `docs/`. **Load [`references/`](references/)** for gRPC, ix tables, flows — hub only.

**Docs revision:** [`references/docs-revision.md`](references/docs-revision.md) — if `docs/` on `main` is past the tracked commit, **tell the user** the skill may be stale; do not silently rewrite skill files.

## pre-alpha disclaimer

- **Exploration only** — not production confidentiality.
- **No real encryption guarantee** — data can be **plaintext on-chain**; do not submit sensitive or real data.
- **Keys / trust model not final**; **devnet resets**; **no warranty**. Do not market as production FHE or private custody to end users.

## references (load on demand)

| file | load for |
| --- | --- |
| [`references/docs-revision.md`](references/docs-revision.md) | `docs/` vs `main` |
| [`references/grpc-api.md`](references/grpc-api.md) | `EncryptService`, proto, clients |
| [`references/instructions.md`](references/instructions.md) | Discriminators, ix groups |
| [`references/frameworks.md`](references/frameworks.md) | Crates, `EncryptCpi`, toolchain |
| [`references/flows.md`](references/flows.md) | Lifecycle, tests, CPI vs signer |

## install & tooling

**TS:** `@encrypt.xyz/pre-alpha-solana-client` + `createEncryptClient` — [`grpc-api.md`](references/grpc-api.md). **Rust** 2024, **Solana CLI** 3.x (`build-sbf`), **Bun**, `just test-unit` / `test-examples` — [`frameworks.md`](references/frameworks.md), [`flows.md`](references/flows.md). Pin git crates per upstream `Cargo.toml`.

## environment (pre-alpha)

| resource | value |
| --- | --- |
| Encrypt gRPC (TLS) | `https://pre-alpha-dev-1.encrypt.ika-network.net:443` |
| Solana RPC | `https://api.devnet.solana.com` (typical) |
| Encrypt program id | `4ebfzWdKnrnGseuQpezXdG8yCdHqwQ1SSBHD3bWArND8` |
| source repo | `https://github.com/dwallet-labs/encrypt-pre-alpha` |

**Canonical:** program id, Encrypt gRPC URL, Solana RPC, git remote — only here; keep samples aligned.

## quick pointers

**On-chain:** first ix byte = **discriminator**; 22 user ix + `emit_event` **228** — [`instructions.md`](references/instructions.md). Common path: discs 1–4 (`create_input_ciphertext` … `execute_graph`); full metas: [instruction reference](https://docs.encrypt.xyz/).

**gRPC:** `encrypt.v1.EncryptService` — `CreateInput`, `ReadCiphertext` — [`grpc-api.md`](references/grpc-api.md).

**Model:** `#[encrypt_fn]` → graph → on-chain `execute_graph` / ciphertext accounts → executor + `commit_ciphertext`; decrypt via gateway ix — [`flows.md`](references/flows.md), [introduction](https://docs.encrypt.xyz/).

## common mistakes

| mistake | instead |
| --- | --- |
| Assuming pre-alpha ciphertexts are secret | Treat as **public / plaintext-capable** (book + repo). |
| Wrong `CreateInput` **authorized** / **network_encryption_public_key** | Match **NetworkEncryptionKey** + access rules — [`grpc-api.md`](references/grpc-api.md). |
| **Encrypt** vs **ika** dWallet | ika signing / `approve_message` → **`ika-solana-prealpha`** skill, not this one. |
| Patching skill when upstream `docs/` changed | **Notify user** — [`docs-revision.md`](references/docs-revision.md). |

**Examples:** [encrypt-pre-alpha `chains/solana/examples`](https://github.com/dwallet-labs/encrypt-pre-alpha/tree/main/chains/solana/examples).
