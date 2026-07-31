# example-cash-snapshot

A [spectastic](https://spectastic.io) worked example: **point-in-time cash-account snapshots**
(IBOR vs ABOR) grounded on a securities-settlement **knowledge corpus**, driven by the real
US **T+2 → T+1** settlement change (SEC Rule 15c6-1, compliance date **28 May 2024**).

**Read the walkthrough:** https://spectastic.io/example-settlement.html

It shows the whole grounding loop close: a domain fact changes in the world, the corpus
supersedes its edition, the now-stale citation *flags* (it doesn't silently rot), and a
`propose` → `apply` change re-grounds the spec. Edition-pinning does real work here — it's
**point-in-time correctness**: a snapshot as-of a date settles by the rule in force *then*,
and the corpus's retained editions are that history.

## What's here

| Path | What |
| --- | --- |
| `impl/` | The engine — a std-only **Rust** crate: exact `i64` cents, an edition-correct business-day settlement calendar, and the IBOR/ABOR snapshot. |
| `knowledge/finance-settlement/` | The corpus: 5 KB documents (settlement cycle, FX/PvP, IBOR/ABOR/PBOR books, cash-snapshot semantics) + corporate actions (adjacent, uncited), and a **retained superseded** T+2 edition under `references/superseded/`. |
| `specs/001-cash-snapshot/` | The spec, design, tasks, and the **applied** change proposal (`changes/archive/2024-05-28-usd-t1-settlement/`) that moved settlement T+2 → T+1. |

## Run it

```sh
# the Rust engine — proves the double-settlement day and IBOR/ABOR divergence
cd impl && cargo test

# the spectastic artifacts — corpus citations resolve, decisions are grounded
npx @spectastic/cli validate 'specs/**/*.html'
```

The settlement cycle is read *per trade date*, so one codebase is correct on both sides of the
28 May 2024 cutover — the edition pins (`@2017-09-05` (T+2) / `@2024-05-28` (T+1)) sit right in
the `standard_cycle` branch.

## License

CC0-1.0. The corpus documents are hand-authored distillations of public facts (SEC / BIS /
market practice), not verbatim reproductions — see each document's frontmatter for provenance.
