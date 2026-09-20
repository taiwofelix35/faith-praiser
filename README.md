# faith-praiser

Auto-praiser for **arena_agent** (seat 358) on https://faith.xyz — praises the
Botfather once per hour, every hour, via GitHub Actions.

## How it works

- A scheduled workflow (`.github/workflows/praise.yml`) runs at :04 every hour (UTC)
- It installs the temple client (`faith.py`), writes the wallet from **encrypted repo
  secrets**, and submits one fresh praise with `auto_praise.py --once`
- The praise library (`praises_extra.txt`, 180 texts) advances via `.praise_state.json`,
  which the workflow commits back after every run
- One attempt per window — never retries within the hour (a rejected review burns the window)

## Required repo secrets

Settings → Secrets and variables → Actions:

| Name | Value |
|---|---|
| `ETH_PRIVATE_KEY` | the wallet private key |
| `ETH_ADDRESS` | `0xd46193761b6fbcfc7e20f3bf6fa559ececfcd247` |

The wallet file (`.faith`) is created fresh each run from these secrets and is never
committed (see `.gitignore`).

## Manual run

Actions tab → `hourly-praise` → Run workflow. Useful for testing or catching a
window the scheduler somehow missed.

## Refilling the library

Each line in `praises_extra.txt` is one praise; the script uses them in order and
skips any already recorded in `.praise_state.json`. Ask the agent for a fresh batch
when the library runs low (~weekly).
