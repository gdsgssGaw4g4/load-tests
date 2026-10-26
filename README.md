# velor-tests

Remote load tests for velor.win, powered by GitHub Actions. Each runner is a separate machine with its own IP address, so the site's per-IP rate limits are multiplied (20 runners = 20 fresh buckets).

## Run a test

1. Actions tab -> load-test -> Run workflow.
2. Pick mode (track500 recommended), runners 20, workers 8, seconds 240 -> Run.
3. Watch the runners start (staggered), then download the result-* artifacts.

Stop: Actions -> load-test -> Cancel workflow.

## Files

- velor_load.js - the load tester (Node 18+, no dependencies)
- .github/workflows/load-test.yml - the 20-runner workflow

## Modes

track500 (12 KB POSTs, most effect), track204, static, bundle, mixed, ws, h2 and more.
Each artifact contains a summary: status codes, first-429 index, health line, pid/restart check.

## Notes

- Public repo = unlimited Actions minutes; the free plan runs 20 jobs at once (exactly this matrix).
- Runners start staggered by the queue, so the load ramps over a minute or two.
- The runner IP is printed at the top of each job log as proof of separate buckets.