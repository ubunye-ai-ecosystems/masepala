# Next steps (paused 2026-09-26)

Work that is waiting, with everything needed to pick it up again.

## 1. The South African runner for SASSA and DWS (waiting on a machine)

**Why:** SASSA and the Department of Water and Sanitation only answer South
African internet addresses. GitHub's own machines are in the United States and
Europe, so they are refused. A machine at home in South Africa, registered as
a GitHub "self-hosted runner", fetches just those two sources every Monday.
No cloud account is involved.

**What is already done:**

- The Monday job (`.github/workflows/za-sources.yml`), the installer
  (`za-runner/setup.sh`) and the guide (`za-runner/README.md`).
- The quarterly refresh uses the copies from the `za-snapshots` branch. Today's
  copies are there (SASSA grants and DWS water quality, fetched 2026-09-26),
  so nothing is missing while the runner is not set up. `/status` shows how
  old they are.
- The repository asks for approval before any outside contributor's workflow
  runs, so a stranger's pull request cannot reach the runner.

**What to do:**

1. Pick an always-on machine at home: a Raspberry Pi 4 or 5, an old laptop or a
   mini PC with Ubuntu. It needs Python 3.11 or newer and git.
2. On a computer where the GitHub CLI is logged in, make a token (valid for one hour):

       gh api -X POST repos/ubunye-ai-ecosystems/masepala/actions/runners/registration-token -q .token

3. On the home machine:

       curl -fsSL https://raw.githubusercontent.com/ubunye-ai-ecosystems/masepala/master/za-runner/setup.sh -o setup.sh
       bash setup.sh <the token>

4. Check GitHub, Settings, Actions, Runners: `masepala-za` should be Idle.
   Then run the job once and check that it passed:

       gh workflow run za-sources.yml -R ubunye-ai-ecosystems/masepala

**If you skip it:** nothing breaks. SASSA publishes monthly and DWS yearly, so
the copies slowly get older, and `/status` marks them once they are more than
120 days old.

## 2. Open items

- **Treasury's API was down** on 2026-09-25 and 2026-09-26. Its sources run on
  their last good copies, and issue #15 (`data-health`) gets a comment on each
  refresh until it is back.
- **Ubunye Engine bug** ubunye-ai-ecosystems/ubunye_engine#95: the lineage store
  mixes up the tasks of one multi-task run. Masepala works around it; the fix
  belongs in the engine repository.
- **News per ward:** news is matched per municipality today. Matching suburb
  names to wards is possible but less reliable; not started.
