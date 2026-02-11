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

## Findings

| # | Repo | Vulnerability | Sev | Pop | Exploit | Proof | Fix | **Total** | Status |
|---|------|--------------|-----|-----|---------|-------|-----|-----------|--------|
| 1 | SPL token-lending | Missing Pyth oracle confidence interval check | 4 | 5 | 4 | 3 | 3 | **720** | **SUBMITTED** |
| 2 | SPL token-lending | Flash loan host_fee_receiver unvalidated | 2 | 5 | 4 | 3 | 3 | **360** | found |
| 3 | MarginFi v2 | StakeStateV2 deserialized without stake program owner check | 4 | 2 | 3 | 2 | 3 | **144** | found |
| 4 | OpenBook v2 | Unchecked i64 arithmetic in order matching | 3 | 2 | 2 | 2 | 3 | **144** | found |
| 5 | SPL token-lending | Liquidation rounding asymmetry (floor vs ceil) | 2 | 5 | 2 | 2 | 3 | **120** | found |
| 6 | MarginFi v2 | Token-2022 transfer hook compatibility gap | 4 | 2 | 3 | 2 | 2 | **96** | found |
| 7 | SPL token-lending | Flash loan @FIXME u64::MAX fee should be inclusive | 2 | 5 | 2 | 2 | 3 | **120** | found |
| 8 | SPL token-lending | Compound interest precision loss (slot-based) | 2 | 5 | 1 | 2 | 2 | **40** | found |
| 9 | MarginFi v2 | Switchboard oracle missing round_open_slot check | 3 | 2 | 2 | 2 | 3 | **72** | found |
| 10 | Anchor | CPI Return<T> unvalidated program ID in return data | 3 | 5 | 2 | 2 | 2 | **120** | found |

## Selected Target

**#1: SPL Token-Lending — Missing Pyth Oracle Confidence Interval Check** (Score: 720)

Rationale:
- Highest score by wide margin (720 vs 360 for #2)
- SPL is the most popular target (4,174 stars)
- Clear vulnerability per Pyth documentation best practices
- High impact: enables undercollateralized borrows during volatile markets
- Clean fix: add confidence interval validation (~10 lines)
- NOT reported upstream (verified: no existing issues)
- Reference implementation = many forks may inherit this vulnerability

## Submission Status

- [x] PoC developed — `tests/pyth_oracle_confidence.rs` (3 test cases)
- [x] Fix implemented — `processor.rs:get_pyth_price()` confidence check (~10 lines)
- [x] Write-up complete — https://gist.github.com/rz1989s/74e2c3d6dc0e2d7ec98f3f22cb631ee8
- [x] PR created — https://github.com/RECTOR-LABS/solana-program-library/pull/1
- [x] API submission sent — Submission ID: `2d8744f3-8888-4810-ac7c-ed8457081de0`
- [ ] Submission verified (pending review)

> **Note:** Upstream `solana-labs/solana-program-library` is archived (read-only).
> PR targets RECTOR-LABS fork. Vulnerability affects all downstream forks.
