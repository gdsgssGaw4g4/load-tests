# load-tests

Remote load tests for velor.win, powered by GitHub Actions. Each runner is a separate machine with its own IP address, so the site's per-IP rate limits are multiplied (20 runners = 20 fresh buckets).

## Run a test

1. Actions tab -> load-test -> Run workflow.
2. Pick mode (track500 recommended), runners 20, workers 8, seconds 240 -> Run.
3. Watch the runners start (staggered), then download the result-* artifacts.

Stop: Actions -> load-test -> Cancel workflow.

## Files

- load.js - the load tester (Node 18+, no dependencies)
- .github/workflows/load-test.yml - the 20-runner workflow

## Modes

track500 (12 KB POSTs, most effect), track204, static, bundle, mixed, ws, h2 and more.
Each artifact contains a summary: status codes, first-429 index, health line, pid/restart check.

## Notes

- Public repo = unlimited Actions minutes; the free plan runs 20 jobs at once (exactly this matrix).
- Runners start staggered by the queue, so the load ramps over a minute or two.
- The runner IP is printed at the top of each job log as proof of separate buckets.
## Schedule

- GitHub: runs automatically 4x/day (cron `0 */6 * * *`, UTC) with defaults (runners 20, workers 8, seconds 120). Edit that single cron line to change. GitHub cron can start a few minutes late; scheduled workflows pause after 60 days of repo inactivity (any push or manual run re-enables).
- Local PC: `pc-wave.ps1` fires a wave straight from this computer. Example hourly task: `schtasks /Create /TN pc-load-wave /SC HOURLY /TR "powershell.exe -NoProfile -ExecutionPolicy Bypass -File <path>\pc-wave.ps1" /F`
- Cloud wave from the PC: `pc-dispatch.ps1` pokes GitHub to run the 20-runner wave (PC only needs to be online for a second).