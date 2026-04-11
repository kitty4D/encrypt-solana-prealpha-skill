# encrypt solana pre-alpha agent skills

**unofficial** agent skill bundle for [Encrypt](https://docs.encrypt.xyz/) on Solana pre-alpha (`skills/encrypt-solana-prealpha/`). Encrypt is the FHE / ciphertext programmability stack (`#[encrypt_fn]`, `execute_graph`, Encrypt gRPC); it is **not** the ika dWallet signing stack—use a separate skill for ika if you are wiring `DWalletService` and `approve_message`.

normative sources: [Encrypt developer guide](https://docs.encrypt.xyz/) and [dwallet-labs/encrypt-pre-alpha](https://github.com/dwallet-labs/encrypt-pre-alpha). if anything here disagrees with those, trust the live docs and repo.

> [!CAUTION]
> pre-alpha has **no real encryption** in the sense of production confidentiality: data can be **plaintext on-chain**, devnet is resettable, and interfaces change. read the disclaimer in the official guide before you ship anything user-facing.

## what's in the box

| path | contents |
| --- | --- |
| `skills/encrypt-solana-prealpha/` | `SKILL.md` + `references/` (including [`references/docs-revision.md`](skills/encrypt-solana-prealpha/references/docs-revision.md)). |

[`docs-revision.md`](skills/encrypt-solana-prealpha/references/docs-revision.md) records which **`docs/`** commit in [encrypt-pre-alpha](https://github.com/dwallet-labs/encrypt-pre-alpha) this bundle was last aligned with. if **`docs/`** on `main` has moved since then, treat the hosted book as ahead of this snapshot. refresh the skill or disable it in your editor until you have a bundle you trust.

## install

the skill directory must stay intact: `SKILL.md` and `references/` as siblings under **`skills/encrypt-solana-prealpha/`**.

### `npx skills` ([skills.sh](https://skills.sh/) / Vercel CLI)

Vercel’s [agent skills guide](https://vercel.com/kb/guide/agent-skills-creating-installing-and-sharing-reusable-agent-context) describes installing from a repo path. replace `OWNER` with your GitHub user or org once this repository is published.

```bash
npx skills add https://github.com/OWNER/encrypt-solana-prealpha-skill/tree/main/skills/encrypt-solana-prealpha
```

add `-g` for a global install when your CLI supports it. see [skills.sh CLI docs](https://skills.sh/docs/cli) and `npx skills --help`.

keep the folder as-is when installing: do not split `SKILL.md` from `references/` or mix in unrelated markdown from other repos.

### Cursor

use [Cursor agent skills](https://cursor.com/docs/context/skills): copy **`skills/encrypt-solana-prealpha/`** into your project or user skills location so the folder name still matches the skill `name` in frontmatter (`encrypt-solana-prealpha`), or point the tool at that path.

### Claude Code

each skill is a directory containing `SKILL.md` (commonly `~/.claude/skills/<name>/` or `.claude/skills/<name>/`). copy **`skills/encrypt-solana-prealpha/`** so `SKILL.md` and `references/` remain siblings.

### other assistants

any tool that loads a skill root plus relative markdown links should work if you pass the folder or `SKILL.md` according to that tool’s docs.

## scope (quick)

FHE DSL and graphs, Encrypt **devnet** program id and **Encrypt gRPC** endpoint (see `SKILL.md`), `EncryptService` (**CreateInput**, **ReadCiphertext**), on-chain instruction groups at a high level (full metas in the official instruction reference), `encrypt-pinocchio` / `encrypt-native` / `encrypt-anchor`, and `@encrypt.xyz/pre-alpha-solana-client`. pre-alpha only: mock executor behavior, resets, not production privacy.

## license and attribution

this repository is licensed under **CC-BY-4.0** (see [`LICENSE`](LICENSE)). that matches how upstream licenses the **mdbook** in [encrypt-pre-alpha](https://github.com/dwallet-labs/encrypt-pre-alpha): prose there is under [**LICENSE-docs** (CC-BY-4.0)](https://github.com/dwallet-labs/encrypt-pre-alpha/blob/main/LICENSE-docs), while **code** there is under [**BSD-3-Clause Clear**](https://github.com/dwallet-labs/encrypt-pre-alpha/blob/main/LICENSE)—the same **documentation vs code** split as [ika-pre-alpha](https://github.com/dwallet-labs/ika-pre-alpha), not a different policy for Encrypt.

this skill text is a third-party summary, not an official dWallet Labs release. when redistributing, keep attribution and see [`NOTICE`](NOTICE). this is not legal advice.
