# load-tests

Remote load tests for velor.win, powered by GitHub Actions. Each runner is a separate machine with its own IP, so the site's per-IP rate limits are multiplied (20 runners = 20 fresh buckets).

## Run a test (manual)

1. Actions tab -> load-test -> Run workflow.
2. Pick mode (track500 recommended), runners 20, workers 8, seconds 240 -> Run.
3. Watch the runners start (staggered), then download the result-* artifacts.

Stop: Actions -> load-test -> Cancel workflow.

## Schedule

- GitHub (automatic, cloud): runs itself every 6 hours - cron `0 */6 * * *` (UTC) with defaults runners 20 / workers 8 / seconds 120. Edit that one line to change. GitHub cron can start a few minutes late; scheduled workflows pause after 60 days of repo inactivity (any push or manual run re-enables).
- Local PC (automatic): a Windows task named `pc-load-wave` runs `pc-wave.ps1` hourly - a 60 s track500 wave from the PC's own IP. Manage it with `schtasks /Run /TN pc-load-wave` or `schtasks /Delete /TN pc-load-wave /F`; edit the params in pc-wave.ps1 to change.

## Files

- load.js - the load tester (Node 18+, no dependencies)
- .github/workflows/load-test.yml - the 20-runner workflow

## Modes

track500 (12 KB POSTs, most effect), track204, static, bundle, mixed, ws, h2 and more. Each artifact contains a summary: status codes, first-429 index, health line, pid/restart check.

## Notes

- Public repo = unlimited Actions minutes; the free plan runs 20 jobs at once (exactly this matrix).
- Runners start staggered by the queue, so the load ramps over a minute or two.
- The runner egress IP is printed at the top of each job log as proof of separate buckets.