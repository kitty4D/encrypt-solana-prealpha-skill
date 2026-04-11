---
name: encrypt-solana-prealpha
description: "Use when building Solana programs with Encrypt pre-alpha (FHE, #[encrypt_fn], EUint types, computation graphs), EncryptService gRPC CreateInput or ReadCiphertext, devnet Encrypt program (execute_graph, ciphertext PDAs, commit_ciphertext, decryption requests), CPI via encrypt-pinocchio / encrypt-native / encrypt-anchor, @encrypt.xyz/pre-alpha-solana-client, encrypt-grpc / encrypt_solana_client, local MockComputeEngine tests, or distinguishing Encrypt from ika dWallet signing flows."
---

# encrypt solana pre-alpha

Normative book: [Encrypt Developer Guide](https://docs.encrypt.xyz/) (mdbook sources in [`encrypt-pre-alpha`](https://github.com/dwallet-labs/encrypt-pre-alpha) `docs/`). **Load [`references/`](references/)** for gRPC shapes, instruction groups, and lifecycle — this file is the hub only.

**Docs revision:** [`references/docs-revision.md`](references/docs-revision.md) — tracked `docs/` commit vs upstream `main`. If `docs/` on `main` has moved past that commit, **tell the user** the skill may be stale; do not silently rewrite skill files to match the book.

## pre-alpha disclaimer (non-negotiable)

Per the [published guide](https://docs.encrypt.xyz/) and [repo README](https://github.com/dwallet-labs/encrypt-pre-alpha):

- **SDK exploration / dev only** — not production confidentiality.
- **No real encryption in pre-alpha** — data can be **plaintext on-chain**; **do not submit sensitive or real data**.
- **Keys and trust model are not final**; **devnet state is wiped** on resets and before Encrypt Alpha 1; **no warranty**.

**Pass-through:** Do not market pre-alpha as production FHE or private custody; surface plaintext/mock limits where end users or the public see the stack.

## references (load on demand)

| file | when to load it |
| --- | --- |
| [`references/docs-revision.md`](references/docs-revision.md) | Stale check for `docs/` on `main` |
| [`references/grpc-api.md`](references/grpc-api.md) | `EncryptService`, `CreateInput`, `ReadCiphertext`, proto + client stubs |
| [`references/instructions.md`](references/instructions.md) | Discriminator groups, executor / gateway / fee ix index |
| [`references/frameworks.md`](references/frameworks.md) | `encrypt-pinocchio`, `encrypt-anchor`, `encrypt-native`, `EncryptCpi`, crates |
| [`references/flows.md`](references/flows.md) | DSL → graph → on-chain → executor → decrypt lifecycle |

## install

**TypeScript (gRPC client):** package **`@encrypt.xyz/pre-alpha-solana-client`** — see [`grpc-api.md`](references/grpc-api.md) for `createEncryptClient` import path.

**Rust:** edition **2024**, **Solana CLI 3.x** (`cargo build-sbf`), **Bun** for TS in upstream repo. Crates live under [`encrypt-pre-alpha`](https://github.com/dwallet-labs/encrypt-pre-alpha) (`crates/`, `chains/solana/`); pin git revisions per upstream `Cargo.toml`.

**Local tests:** upstream `just test-unit`, `just test-examples` — see repo `justfile` and [`flows.md`](references/flows.md).

## environment (pre-alpha)

| resource | value |
| --- | --- |
| Encrypt gRPC (TLS) | `https://pre-alpha-dev-1.encrypt.ika-network.net:443` |
| Solana RPC | `https://api.devnet.solana.com` (typical) |
| Encrypt program id | `4ebfzWdKnrnGseuQpezXdG8yCdHqwQ1SSBHD3bWArND8` |
| source repo | `https://github.com/dwallet-labs/encrypt-pre-alpha` |

**Canonical:** Program id, Encrypt gRPC URL, default Solana RPC, and git remote live only here; samples elsewhere must match.

## wire quick pointers

- **On-chain program:** first ix data byte = **discriminator** — 22 user instructions + `emit_event` (**228**) — [`instructions.md`](references/instructions.md).
- **Executor path (common):** `create_input_ciphertext` (1), `create_plaintext_ciphertext` (2), `commit_ciphertext` (3), `execute_graph` (4), … — full metas in the [book instruction reference](https://docs.encrypt.xyz/).
- **gRPC:** `encrypt.v1.EncryptService` — **`CreateInput`**, **`ReadCiphertext`** — [`grpc-api.md`](references/grpc-api.md).

## core model

1. Author FHE logic with **`#[encrypt_fn]`** → macro builds a **computation graph** (DAG).
2. **On-chain** **`execute_graph`** (and related ix) creates/updates **ciphertext accounts** and emits events.
3. **Off-chain executor** (pre-alpha mock) evaluates FHE, then **`commit_ciphertext`** moves pending → verified.
4. **Decryption** via gateway ix (`request_decryption` / `respond_decryption`) when plaintext is needed — see [on-chain docs](https://docs.encrypt.xyz/).

Detail: [`flows.md`](references/flows.md) and the [introduction](https://docs.encrypt.xyz/).

## workflows

End-to-end patterns, testing, and CPI vs signer paths: [`flows.md`](references/flows.md).

Instruction account metas and data layouts: [published instruction reference](https://docs.encrypt.xyz/) (mirror in repo `docs/src/reference/instructions.md`).

## common mistakes

| mistake | what to do instead |
| --- | --- |
| **Assuming pre-alpha ciphertexts are secret on-chain** | Treat as **public / plaintext-capable**; read the disclaimer in the book and repo. |
| **Skipping `authorized` / `network_encryption_public_key` alignment on CreateInput** | Match on-chain **NetworkEncryptionKey** and program **authorized** semantics — [`grpc-api.md`](references/grpc-api.md), [access control](https://docs.encrypt.xyz/). |
| Confusing **Encrypt** with **ika dWallet** pre-alpha | **Encrypt** = FHE on ciphertexts + executor + decrypt path. **ika** = programmable signing / MPC-style mock — use the **`ika-solana-prealpha`** skill for dWallet gRPC and `approve_message`, not this one. |
| **Patching this skill** when upstream `docs/` changed | Per [`docs-revision.md`](references/docs-revision.md): **notify the user**; do not silently edit the bundle. |

## related material (optional)

- [encrypt-pre-alpha examples](https://github.com/dwallet-labs/encrypt-pre-alpha/tree/main/chains/solana/examples) (voting, counter, CP-Token, CP-Swap, ACL, coin-flip; Pinocchio / Native / Anchor variants).
- Generic Solana program skills for non-Encrypt CPI and account patterns.
