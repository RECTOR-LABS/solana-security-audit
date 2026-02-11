# Vulnerability Scoreboard

## Scoring Formula

```
TOTAL = SEVERITY(1-5) x POPULARITY(1-5) x EXPLOITABILITY(1-5) x PROOF_QUALITY(1-3) x FIX_SIMPLICITY(1-3)
```

Max possible: 1,125

## Selection Criteria

1. Highest total score
2. Clean, targeted fix (minimal diff)
3. Clear PoC demonstrating the exploit
4. Popular repo (stars matter for judging)
5. NOT already reported upstream
6. **Repo must NOT be archived or stale** (upstream PR required, active development)

## Eliminated Findings

**solana-program-library** (solana-labs) — **archived** March 2025:
- ~~Missing Pyth oracle confidence interval check (score 720)~~
- ~~Flash loan host_fee_receiver unvalidated (score 360)~~
- ~~Liquidation rounding asymmetry (score 120)~~
- ~~Flash loan @FIXME u64::MAX fee (score 120)~~
- ~~Compound interest precision loss (score 40)~~

**openbook-v2** (openbook-dex) — **stale** last commit June 2024 (~2 years):
- ~~Unchecked i64 arithmetic in order matching (score 144)~~

## Active Findings

| # | Repo | Vulnerability | Sev | Pop | Exploit | Proof | Fix | **Total** | Status |
|---|------|--------------|-----|-----|---------|-------|-----|-----------|--------|
| 1 | MarginFi v2 | StakeStateV2 deserialized without stake program owner check | 4 | 2 | 3 | 2 | 3 | **144** | found |
| 2 | Anchor | CPI Return<T> unvalidated program ID in return data | 3 | 5 | 2 | 2 | 2 | **120** | found |
| 3 | MarginFi v2 | Token-2022 transfer hook compatibility gap | 4 | 2 | 3 | 2 | 2 | **96** | found |
| 4 | MarginFi v2 | Switchboard oracle missing round_open_slot check | 3 | 2 | 2 | 2 | 3 | **72** | found |

## Selected Target

**TBD** — Need to select from active findings and develop PoC + upstream PR.

Top candidate: **#1 MarginFi v2 — StakeStateV2 owner check** (Score: 144)
- Active repo (pushed Feb 10, 2026)
- High severity (oracle price manipulation)
- Clean fix (~3 lines)
- Can create upstream PR to mrgnlabs/marginfi-v2

## Submission Status

- [x] Previous submission (SPL) — INVALIDATED (archived repo)
- [ ] New target selected
- [ ] PoC developed
- [ ] Fix implemented
- [ ] Write-up complete
- [ ] Upstream PR created
- [ ] API submission updated
