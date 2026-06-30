# elip-silent-payments-liquid — reference implementations

Reference implementations for [`elip-silent-payments-liquid.mediawiki`](../elip-silent-payments-liquid.mediawiki),
following the layout convention of [bitcoin/bips](https://github.com/bitcoin/bips)
(e.g. `bip-0352/`): supporting code lives in a folder named after the ELIP.

Two independent implementations derive the same test vectors byte-for-byte:

- [`rust/`](rust) — built on `lwk_wollet`. `cargo test` to run.
- [`python/`](python) — in the canonical BIP-352 `reference.py` style, using the
  pure-Python `secp256k1lab` for the curve algebra and `wallycore` only for the
  Liquid Confidential Transactions plumbing. See [`python/README.md`](python/README.md).

Both cover:

- **Address encoding** — Bech32m, HRP `lqsp`/`tlqsp`, version `q`
- **Sender derivation** — input aggregation, ECDH shared secret, `P_k`, `BK_k`, `bk_k`
- **Tweak server** — `T = input_hash · A`, publish, and client-side `S = b_scan · T`
- **Receiver scanning** — recompute `P_k`, match against outputs, derive spend secret
- **Confidential output blinding and unblinding** — the ELIP's novel claim: `bk_k`
  derived from the shared secret unblinds the output non-interactively
- **Test vectors** — deterministic known-answer values matching the ELIP specification

## License

BSD-3-Clause.
