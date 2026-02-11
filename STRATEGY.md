# Audit Strategy

## Rule: No Archived or Stale Repos

Never target archived or stale repositories — PRs to inactive repos won't get reviewed, and archived repos block PRs entirely.

## High-Value Targets (ranked by probability)

1. **MarginFi v2 StakeStateV2 owner check** — deserialization without stake program owner validation
2. **MarginFi v2 Token-2022 transfer hook gap** — transfer hooks not accounted for
3. **MarginFi v2 Switchboard oracle staleness** — missing round_open_slot check
4. **Anchor CPI Return<T>** — unvalidated program ID in return data

## Priority Order Rationale

MarginFi v2 is #1 because:
- Active repo (pushed Feb 10 2026)
- 3 findings identified (highest count after eliminated SPL)
- Lending protocol = high-impact vulnerability class
- Can create upstream PR (not archived)

## Target Repositories (4 active)

| # | Repository | Default Branch | Active | Stars |
|---|-----------|---------------|--------|-------|
| 1 | marginfi-v2 (mrgnlabs) | `main` | Yes (Jan 2026) | 284 |
| 2 | anchor (coral-xyz) | `master` | Yes (Jan 2026) | 4,949 |
| 3 | mpl-token-metadata (metaplex-foundation) | `main` | Yes (Jan 2026) | 243 |
| 4 | jito-solana (jito-foundation) | `master` | Yes (Feb 2026) | 681 |

**Eliminated**:
- solana-program-library (solana-labs) — ARCHIVED March 2025
- openbook-v2 (openbook-dex) — STALE, last commit June 2024 (~2 years)

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
| Repo is archived/stale | CHECK BEFORE SELECTING — verify with `gh repo view --json isArchived` + check last commit date |
| Finding already reported upstream | Check open issues + closed PRs before selecting |
| Fix breaks existing tests | Run full test suite, iterate on fix |
| Time crunch | Cut iterations 6-7, focus on 1-4 (highest yield) |
| API submission fails | Backup: manual submission or skill retry |
