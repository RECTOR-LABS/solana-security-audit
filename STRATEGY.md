# Audit Strategy

## High-Value Targets (ranked by probability)

1. **SPL token-lending flash loan fee rounding** — fee rounds DOWN = drain fee reserve
2. **SPL token-lending interest accrual precision loss** — compounding over many slots loses precision
3. **SPL token-lending liquidation bonus overflow** — large collateral values overflow
4. **SPL token-lending oracle staleness** — stale prices enable unfair liquidation
5. **SPL flash loan callback validation** — arbitrary CPI to receiver program
6. **OpenBook i80f48 overflow** — fixed-point math in order matching
7. **MarginFi two-step liquidation race** — state change between start/end

## Priority Order Rationale

SPL token-lending is #1 because:
- 4,174 stars (high popularity score)
- UNAUDITED lending protocol (rare for this codebase size)
- Complex math (decimal.rs, rate.rs) with known classes of arithmetic bugs
- Flash loan mechanics = rich attack surface

## Time Budget

| Day | Phase | Hours | Activity |
|-----|-------|-------|----------|
| Feb 11 (PM) | 1 | 3h | Setup: meta repo, forks, clones, labels, milestones |
| Feb 12 | 2 | 8h | Iterations 1-4 (account, signer, PDA, arithmetic) |
| Feb 13 | 2+3 | 8h | Iterations 5-7 + scoring + target selection |
| Feb 14 | 4 | 8h | Deep-dive: PoC, fix, write-up, PR |
| Feb 15 (AM) | 5 | 2h | Final review, upstream PR, API submission |

**Submit by**: Feb 15 12:00 UTC (6h buffer before deadline)

## Risk Mitigation

| Risk | Mitigation |
|------|------------|
| No findings in SPL token-lending | Pivot to OpenBook i80f48 math or marginfi |
| Finding already reported upstream | Check open issues + closed PRs before selecting |
| Fix breaks existing tests | Run full test suite, iterate on fix |
| Time crunch | Cut iterations 6-7, focus on 1-4 (highest yield) |
| API submission fails | Backup: manual submission or skill retry |
