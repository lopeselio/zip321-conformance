# zip321-conformance

> **Status: planned.** The plan below and the [issues](https://github.com/lopeselio/zip321-conformance/issues) track the work.

Shared test vectors for [ZIP-321](https://zips.z.cash/zip-0321) payment requests, and a report of how each
implementation handles them. Wallets and libraries that read `zcash:` links should agree on what is valid and what it
means; this project measures whether they do.

## Plan

1. **Vectors.** A versioned JSON file of cases, each with a URI, whether it is valid, and either the expected payments or
   the expected error (and the parameter it concerns). Seed it from the ZIP's own examples, the reference Rust
   [`zip321`](https://crates.io/crates/zip321) crate's tests, and the cases in
   [zcash-zip321](https://github.com/lopeselio/zcash-zip321) (duplicate parameters, leading-zero indices, more than 8
   decimals, amounts above the supply cap, memos to transparent addresses, mixed networks, unknown `req-` parameters,
   `req-asset`, …).
2. **Runners.** Small adapters that feed every vector to an implementation and record the result:
   - the Rust `zip321` crate (reference);
   - [zcash-zip321](https://github.com/lopeselio/zcash-zip321);
   - zcash.js, once it supports ZIP-321;
   - Thus Spoke Zakura's `/api/v1/zip321/parse` endpoint.
3. **Report.** A generated table: which implementations accept, reject, or misread each case, published as a page.
4. **Wallets.** A manual checklist for mobile and desktop wallets (scan a QR, record what the wallet shows), since
   wallets can't be driven by a script. Multi-payment support and URI length limits matter most to
   [zcash-batch-payouts](https://github.com/lopeselio/zcash-batch-payouts).
5. **Upstream.** Report disagreements to the implementations, and errata to the ZIP. One is already known: the ZIP's
   second `req-asset` example is 74 bytes rather than 75.

## Format (draft)

```json
{
  "id": "amount-too-many-decimals",
  "uri": "zcash:…?amount=1.123456789",
  "valid": false,
  "error": { "code": "INVALID_AMOUNT", "param": "amount" },
  "source": "ZIP-321 §amount"
}
```

Error codes start as zip321-js's codes and will be mapped per implementation, since each names its errors differently.

MIT licensed. Vectors taken from other projects keep their original licences, noted next to them.
