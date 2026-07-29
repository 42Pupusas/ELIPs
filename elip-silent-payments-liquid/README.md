# elip-silent-payments-liquid — reference implementation

Reference implementation for [`elip-silent-payments-liquid.mediawiki`](../elip-silent-payments-liquid.mediawiki),
following the layout of [bitcoin/bips](https://github.com/bitcoin/bips) `bip-0352/`:
a flat script next to the ELIP, in the style of the canonical
[BIP-352 reference](https://github.com/bitcoin/bips/blob/master/bip-0352/reference.py).

- [`reference.py`](reference.py) — the protocol: addresses, input aggregation,
  shared secret, per-output derivation, tweak server, and Confidential
  Transactions blinding/unblinding. Elliptic-curve algebra uses the pure-Python
  `secp256k1lab` (same library the BIP-352 reference uses); the CT-specific
  functions use `wallycore`.
- [`bech32m.py`](bech32m.py) — Bech32m, verbatim from BIP-350 / the BIP-352
  reference.
- [`test_vectors.py`](test_vectors.py) — byte-pinned test vectors plus
  taproot/tweak-server/address/CT/label/peg-in round-trip tests.

Covers:

- **Address encoding** — Bech32m, HRP `lqsp`/`tlqsp`, version `q`, including
  explicit testnet/regtest (`tlqsp`, shared per the ELIP) and mainnet separation
- **Sender derivation** — input aggregation, ECDH shared secret, `P_k`, `BK_k`, `bk_k`
- **Labels** — `B_m = B_spend + label_tweak(b_scan, m)·G`, unchanged from BIP-352;
  sender/receiver spend-key agreement for a labeled address
- **Peg-in input eligibility** — a peg-in contributes no pubkey to the shared
  secret but its outpoint still participates in `outpoint_L` selection
- **Tweak server** — `T = input_hash · A`, publish, and client-side `S = b_scan · T`
- **Receiver scanning** — recompute `P_k`, match against outputs, derive spend secret
- **Confidential output blinding and unblinding** — the ELIP's novel claim: `bk_k`
  derived from the shared secret unblinds the output non-interactively

## Running

```sh
pip install pytest 'secp256k1lab @ git+https://github.com/secp256k1lab/secp256k1lab.git' wallycore
pytest -v elip-silent-payments-liquid/test_vectors.py
```

`secp256k1lab` is fetched from git (not published on PyPI). `wallycore`'s PyPI
wheels are built with Elements support, which the CT test needs; if it is not
installed, that one test is skipped automatically.

## License

BSD-3-Clause.
