# Audit Strategy

## Rule: No Archived or Stale Repos

Never target archived or stale repositories — PRs to inactive repos won't get reviewed, and archived repos block PRs entirely.

## High-Value Targets (ranked by probability)

1. **Anchor CPI Return<T>** — unvalidated program ID in return data (**CONFIRMED**, Score 540)
2. ~~MarginFi v2 StakeStateV2 owner check~~ — FALSE POSITIVE
3. ~~MarginFi v2 Token-2022 transfer hook gap~~ — FALSE POSITIVE
4. ~~MarginFi v2 Switchboard oracle staleness~~ — FALSE POSITIVE

## Priority Order Rationale

Anchor F2 is #1 because:
- **CONFIRMED vulnerability** — all MarginFi findings were false positives
- Framework-level impact (4,949 stars, affects entire Solana ecosystem)
- Score 540 (highest by far after rescoring)
- Active repo (pushed Jan 25, 2026)
- Internal evidence: same repo's `token_2022.rs` implements correct pattern
- Clean fix path: add program_id validation to `Return<T>::get()`

## Target Repositories (14 active)

### Tier 1 — Original (audited)

| # | Repository | Upstream | Default Branch | Last Push | Stars |
|---|-----------|----------|---------------|-----------|-------|
| 1 | marginfi-v2 | mrgnlabs | `main` | 2026-01-28 | 284 |
| 2 | anchor | coral-xyz | `master` | 2026-01-25 | 4,949 |
| 3 | mpl-token-metadata | metaplex-foundation | `main` | 2026-01-15 | 243 |
| 4 | jito-solana | jito-foundation | `master` | 2026-02-10 | 681 |

### Tier 2 — Expanded (high-value DeFi/infra)

| # | Repository | Upstream | Default Branch | Last Push | Stars |
|---|-----------|----------|---------------|-----------|-------|
| 5 | wormhole | wormhole-foundation | `main` | 2026-02-10 | 1,875 |
| 6 | agave | anza-xyz | `master` | 2026-02-11 | 1,669 |
| 7 | pinocchio | anza-xyz | `main` | 2026-02-11 | 849 |
| 8 | whirlpools | orca-so | `main` | 2026-02-10 | 509 |
| 9 | protocol-v2 | drift-labs | `master` | 2026-02-11 | 376 |
| 10 | raydium-clmm | raydium-io | `master` | 2025-12-29 | 368 |

### Tier 3 — Additional

| # | Repository | Upstream | Default Branch | Last Push | Stars |
|---|-----------|----------|---------------|-----------|-------|
| 11 | phoenix-v1 | Ellipsis-Labs | `master` | 2026-02-04 | 246 |
| 12 | pyth-crosschain | pyth-network | `main` | 2026-02-10 | 226 |
| 13 | squads-v4 | Squads-Protocol | `main` | 2026-02-09 | 171 |
| 14 | klend | Kamino-Finance | `master` | 2026-02-09 | 158 |

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
