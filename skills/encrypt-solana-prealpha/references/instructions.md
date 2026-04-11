# instructions snapshot (Encrypt Solana)

**Canonical tables and account metas:** [Instruction reference](https://docs.encrypt.xyz/reference/instructions.html) (repo: `docs/src/reference/instructions.md`).

**Rule:** First byte of instruction data = **discriminator**. The program defines **22** functional instructions plus **`emit_event` (discriminator 228)**.

---

## groups (quick index)

| group | disc range | instructions |
| --- | --- | --- |
| Setup | 0 | `initialize` |
| Executor | 1–6 | `create_input_ciphertext`, `create_plaintext_ciphertext`, `commit_ciphertext`, `execute_graph`, `register_graph`, `execute_registered_graph` |
| Ownership | 7–9 | `transfer_ciphertext`, `copy_ciphertext`, `make_public` |
| Gateway | 10–12 | `request_decryption`, `respond_decryption`, `close_decryption_request` |
| Fees | 13–18 | `create_deposit`, `top_up`, `withdraw`, `update_config_fees`, `reimburse`, `request_withdraw` |
| Authority | 19–21 | `add_authority`, `remove_authority`, `register_network_encryption_key` |
| Event | 228 | `emit_event` |

---

## CPI vs signer

Several instructions (**e.g.** `create_plaintext_ciphertext`, `execute_graph`) support **signer** and **CPI** account layouts — the CPI path inserts a **`cpi_authority`** account and shifts later indices. Always copy metas from the live book for the path you use.

---

## when to read full reference

Load the published instruction chapter when wiring **exact account order**, **writable/signer flags**, and **variable-length data** (graph bytes, plaintext payloads, fee configs).
