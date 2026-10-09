# prime-counting

The prime counting bound `π(x) ≤ 4x / (3 log x)` of IUT IV, Proposition 1.6.

## Status

**Not started.** The repository is the initial project scaffold (a single commit,
2026-07-20). The library `PrimeCounting` contains only the placeholder module
`PrimeCounting/Basic.lean`, which has no declarations: there are no definitions or theorems yet
(and so no `sorry`, `axiom` or `admit`). Everything under *Goals* below is planned, not
formalised.

No lana-agents repository depends on this one: `iut` and its pinned lana-agents
dependencies do not `require` it.

`iut` no longer needs this repository. It uses Proposition 1.6 with the factor `3/2`
instead of `4/3`, and that bound is proved from Mathlib's Chebyshev bound
`θ(x) ≤ (log 4)·x`: `Iut.primeCountingHyp_holds` and `Iut.primeCountingBoundExplicit`
(`iut/Iut/Implication/ChebyshevExplicit.lean`). The `4/3` bound remains open in the
lana-agents repositories.

## Goals

Prove the bound of IUT IV Proposition 1.6 with the printed factor — the eventual bound

```
π(x) ≤ 4x / (3 log x)
```

Mathlib v4.32.0 only has Chebyshev-strength bounds, whose coefficient `log 4` exceeds the required `4/3`. PNT-strength results do exist in Lean outside Mathlib, notably `PrimeNumberTheoremAnd` (Kontorovich–Tao et al.).

The task is to integrate that development — by `require` or by upstreaming — so that the bound is proved unconditionally.

## Related repositories

No longer needed by `iut` (see *Status*). The exact `4/3` statement remains in `iut` only
as the comparator challenge `Iut4Sec1.eventually_primeCounting_le_four_thirds`
(`Comparator/Challenge.lean`), which is outside the `iut` library.

## Layout

Lean 4 project pinned to `leanprover/lean4:v4.32.0` with Mathlib at `v4.32.0`
(commit `81a5d25`), its only direct dependency.
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

License: Apache 2.0 (see LICENSE).
