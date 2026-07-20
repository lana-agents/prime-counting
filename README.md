# prime-counting

PNT-strength prime counting bounds to discharge the PrimeCountingCertificate.

## Goals

Discharge the `PrimeCountingCertificate` that IUT4 §1 uses to state Proposition 1.6 — the eventual bound

```
π(x) ≤ 4x / (3 log x)
```

Mathlib v4.32.0 only has Chebyshev-strength bounds, whose coefficient `log 4` exceeds the required `4/3`. PNT-strength results do exist in Lean outside Mathlib, notably `PrimeNumberTheoremAnd` (Kontorovich–Tao et al.).

The task is to integrate that development — by `require` or by upstreaming — so that the certificate is discharged unconditionally.

Per the project owner, `iut` itself only *states* the result; no proving effort happens there.

## Related repositories

Discharges a certificate interface used by `iut`.

## Layout

Lean 4 project pinned to `leanprover/lean4:v4.32.0` with Mathlib at `v4.32.0`.
Library sources live under `PrimeCounting/`, and every file must be imported from
the root module `PrimeCounting.lean`.

```bash
lake exe cache get                       # fetch the Mathlib build cache
lake build                               # build the library
lake exe mk_all --lib PrimeCounting --git   # regenerate the root module after adding files
```

## Validation

`.orchestra/` tells the agent harness how to prepare the environment and how to
check that a change is complete:

* `before.sh` warms the Mathlib build cache before work starts.
* `validation.sh` checks the worktree is clean, that every `.lean` file is
  imported (`mk_all --check`), and that everything builds with warnings as
  errors (`lake build --wfail`).

Run it locally with `bash .orchestra/validation.sh`.

## Tracker

Work is tracked in taxis: [#6](https://taxis.lana.merten.dev/issues/6)
