# ci-matrix-test

Sandbox repo to validate two behaviors used by
[open-vela/public-actions#58](https://github.com/open-vela/public-actions/pull/58):

1. **Matrix parallelism with `fail-fast: false`** — one failing group does
   not cancel the others.
2. **`needs.<job>.result` aggregation** — for a matrix job, `.result` is
   GitHub's built-in aggregate: `success` iff every matrix instance
   succeeded. This is the correct replacement for reading a matrix job's
   `outputs.status`, which gets overwritten by each instance.

## How to run

1. Go to the [Actions tab](https://github.com/liujinye-sys/ci-matrix-test/actions).
2. Pick **Matrix aggregation test** and click **Run workflow**.
3. Leave `force_fail_group` empty → expect 5 green matrix runs,
   post-processing prints `ci_status=success`.
4. Run again with `force_fail_group=qemu` → expect `qemu` red and the
   other 4 groups still green (thanks to `fail-fast: false`);
   post-processing prints `ci_status=failure`.

The workflow mirrors the matrix shape of
`open-vela/public-actions/.github/workflows/ci.yml` (groups: goldfish,
qemu, sil, aurix, flagchip) but replaces real builds with `echo` so it
runs in seconds and needs no secrets.
