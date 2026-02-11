# Audit Tracker

## Progress

| # | Repo | Upstream | Stars | Branch | Status | Findings |
|---|------|----------|-------|--------|--------|----------|
| 1 | marginfi-v2 | [mrgnlabs/marginfi-v2](https://github.com/mrgnlabs/marginfi-v2) | 284 | `main` | done | 0 (3 false positives) |
| 2 | anchor | [coral-xyz/anchor](https://github.com/coral-xyz/anchor) | 4,949 | `master` | done | 1 (F2 submitted) |
| 3 | mpl-token-metadata | [metaplex-foundation/mpl-token-metadata](https://github.com/metaplex-foundation/mpl-token-metadata) | 243 | `main` | done | 0 (pristine) |
| 4 | jito-solana | [jito-foundation/jito-solana](https://github.com/jito-foundation/jito-solana) | 681 | `master` | done | 0 (2 minor operational) |
| 5 | wormhole | [wormhole-foundation/wormhole](https://github.com/wormhole-foundation/wormhole) | 1,875 | `main` | done | 0 (2 informational) |
| 6 | agave | [anza-xyz/agave](https://github.com/anza-xyz/agave) | 1,669 | `master` | done | 1 (F10 DoS) |
| 7 | pinocchio | [anza-xyz/pinocchio](https://github.com/anza-xyz/pinocchio) | 849 | `main` | done | 3 (F7-F9 defensive) |
| 8 | whirlpools | [orca-so/whirlpools](https://github.com/orca-so/whirlpools) | 509 | `main` | done | 0 (by-design wrapping) |
| 9 | protocol-v2 | [drift-labs/protocol-v2](https://github.com/drift-labs/protocol-v2) | 376 | `master` | done | 2 (F12-F13 low) |
| 10 | raydium-clmm | [raydium-io/raydium-clmm](https://github.com/raydium-io/raydium-clmm) | 368 | `master` | done | 2 (F3-F4 admin-gated) |
| 11 | phoenix-v1 | [Ellipsis-Labs/phoenix-v1](https://github.com/Ellipsis-Labs/phoenix-v1) | 246 | `master` | done | 3 (F5-F6, F11) |
| 12 | pyth-crosschain | [pyth-network/pyth-crosschain](https://github.com/pyth-network/pyth-crosschain) | 226 | `main` | done | 0 (1 false positive) |
| 13 | squads-v4 | [Squads-Protocol/v4](https://github.com/Squads-Protocol/v4) | 171 | `main` | done | 0 (pristine) |
| 14 | klend | [Kamino-Finance/klend](https://github.com/Kamino-Finance/klend) | 158 | `master` | done | 1 (F14 low) |

**Legend**: pending | **IN PROGRESS** | done | skipped
**Sweep completed**: Feb 11, 2026 — all 14 repos audited

## Summary Statistics

- **Repos audited**: 14/14
- **Real findings**: 13 (across 7 repos)
- **False positives eliminated**: 11+
- **Clean repos**: 5 (Squads, Metaplex, Whirlpools, Jito, Wormhole)
- **Submitted**: Anchor F2 (score 540)

## Existing Findings (Ranked by Score)

| # | Repo | Vulnerability | Score | Status |
|---|------|--------------|-------|--------|
| F2 | Anchor | CPI Return<T> unvalidated program ID | 540 | **SUBMITTED** |
| F5 | Phoenix | Ask-side fee underflow | 216 | Verified |
| F6 | Phoenix | Quote lot validation overflow | 162 | Verified |
| F10 | Agave | Vote state increment_credits unwrap | 144 | DoS only |
| F7 | Pinocchio | Unbounded align_offset (lazy entrypoint) | 108 | Defensive |
| F8 | Pinocchio | Instructions sysvar buffer underflow | 108 | unchecked |
| F9 | Pinocchio | Unvalidated instruction offset | 108 | unchecked |
| F3 | Raydium | Fee rate sum bypass | 108 | Admin-gated |
| F4 | Raydium | Trade fee rate underflow | 72 | Admin-gated |
| F11 | Phoenix | Multi-order balance underflow | 72 | Edge case |
| F12 | Drift | Confidence interval inflation | 64 | Design |
| F13 | Drift | Unsafe unwrap oracle deser | 48 | DoS |
| F14 | Klend | Division by Zero in LTV | 36 | Low |

## Audit Log

### #1 marginfi-v2 — DONE
- **Completed**: Feb 11, 2026
- **Result**: 0 exploitable. F1, F3, F4 false positives.

### #2 anchor — DONE
- **Completed**: Feb 11, 2026
- **Finding**: F2 — CPI Return<T> (CONFIRMED, SUBMITTED)
- **Issue**: https://github.com/solana-foundation/anchor/issues/4232
- **PR**: https://github.com/solana-foundation/anchor/pull/4231 (Fixes #4232)
- **Tests**: 13/13 passing, cargo check 3/3 clean

### #3 mpl-token-metadata — DONE
- **Completed**: Feb 11, 2026
- **Result**: 0 vulnerabilities. Excellent security engineering.

### #4 jito-solana — DONE
- **Completed**: Feb 11, 2026
- **Result**: 0 exploitable. 2 minor operational (unwrap calls, rollback handling).

### #5 wormhole — DONE
- **Completed**: Feb 11, 2026
- **Result**: 0 exploitable. 2 informational (decimal overflow, off-by-one self-DoS).

### #6 agave — DONE
- **Completed**: Feb 11, 2026
- **Finding**: F10 — Vote state unwrap panic (DoS, sev 3, exploit 2)

### #7 pinocchio — DONE
- **Completed**: Feb 11, 2026
- **Findings**: F7-F9 — Defensive coding issues in lazy entrypoint and Instructions sysvar

### #8 whirlpools — DONE
- **Completed**: Feb 11, 2026
- **Result**: 0 exploitable. By-design wrapping arithmetic (Uniswap v3 pattern).

### #9 protocol-v2 (Drift) — DONE
- **Completed**: Feb 11, 2026
- **Findings**: F12 confidence inflation (design), F13 unsafe unwrap (DoS)

### #10 raydium-clmm — DONE
- **Completed**: Feb 11, 2026
- **Findings**: F3-F4 admin-gated fee validation bugs

### #11 phoenix-v1 — DONE
- **Completed**: Feb 11, 2026
- **Findings**: F5 fee underflow (216), F6 overflow (162), F11 balance tracking (72)

### #12 pyth-crosschain — DONE
- **Completed**: Feb 11, 2026
- **Result**: 0 exploitable. 1 false positive (permissionless relay by design).

### #13 squads-v4 — DONE
- **Completed**: Feb 11, 2026
- **Result**: 0 vulnerabilities. Production-ready.

### #14 klend — DONE
- **Completed**: Feb 11, 2026
- **Finding**: F14 — Division by Zero in LTV (low impact)

## Eliminated Repos

- solana-program-library (solana-labs) — archived March 2025
- openbook-v2 (openbook-dex) — stale, last commit June 2024
