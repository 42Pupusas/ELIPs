# Rust reference implementation

Reference implementation for the *Silent Payments for the Liquid Network* ELIP.
Each function maps to a single derivation step from the specification. The only
external dependency is [`lwk_wollet`](https://crates.io/crates/lwk_wollet), used
for secp256k1 primitives, key and scalar types, tagged hashes, Taproot script
construction, and Confidential Transactions primitives. All Silent Payments logic
is implemented directly from the ELIP.

## Covers

- **Address encoding** — Bech32m, HRP `lqsp`/`tlqsp`, version `q`
- **Sender derivation** — input aggregation, ECDH shared secret, `P_k`, `BK_k`, `bk_k`
- **Tweak server** — `T = input_hash · A`, publish, and client-side `S = b_scan · T`
- **Receiver scanning** — recompute `P_k`, match against outputs, derive spend secret
- **Confidential output blinding and unblinding** — the ELIP's novel claim: `bk_k`
  derived from the shared secret unblinds the output non-interactively
- **Test vectors** — deterministic known-answer values matching the ELIP specification

## Run

```sh
# 5 tests: test_vectors, taproot_even_y_negation, tweak_server_agreement,
#          ct_round_trip_unblind_with_bk, address_round_trip_and_network_separation
cargo test
```

## License

BSD-3-Clause.
