# Owner release qualification

`codex/sync.py` owns the enrolled updater; `.github/workflows/test.yml` supplies its remote CI proof. The added job uses the same isolated `codex/check.py --full` gate (core, dashboard, release validation). Native Windows checks and upstream jobs remain required. GitHub API and push evidence identify `medking82/hermes-jev-skills` as a redirect to `medking82/metis-agent` (repository ID `1424351713`): the SOP and standalone checkouts have distinct local origins but share one hosted fork. Their qualification branches are kept distinct. Existing updater repository enrollment and CI provenance must still match the configured origin; this change does not silently re-enroll it.

Full offline wrapper; suite timing only. Reuse disabled; no updater enrollment changes.

The runner is `medking82/hermes-fleet-sync` at
`4f79a5b4452d9a91037e4cbf564a927b44e2bac6` (branch
`perf/shared-release-qualification`). `scripts/qualify-release.py` reads the exact
committed manifest, source commit, and pinned engine blobs through Git. It never
fetches or edits the shared checkout. Dirty source bytes are not release evidence.
Receipts retain runner, source/tree/lock, environment/package, command, scope,
coverage, log and freshness identities; failed or stale evidence cannot be reused.
No local receipt substitutes for the owner's CI, ancestry, authorization, or locks.

Run with native Linux Python/Node and state on the WSL filesystem, for example:

```sh
python3 -B scripts/qualify-release.py \
  --runner /mnt/c/GitHub/hermes-release-opt \
  --state "$HOME/sop-release-qualification/metis-agent"
```

Install the same prerequisites as the existing gate first. Python unittest suites
may use `--class-shards N --timings /absolute/path/prior-receipt.json`; custom and
Node commands remain serial. Keep one shard unless a measured comparison proves
benefit with the same covered tests. Receipts include whole-class measurements
and group weights; optional packages/skips remain visible in the suite logs.

`--timings` is a read-only placement input. Preserve that file across repeated
runs and write results to the runner's separate receipt path. To refresh weights,
explicitly choose a new prior receipt; never overwrite placement input after each
success. `--reuse` is allowed only where the committed lane enables local reuse,
with the exact same weights and identity, intact logs, and a fresh success. Each
CI runner starts with fresh native state; CI does not request reuse.
