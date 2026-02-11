# Solana Security Audit

Systematic security audit of open-source Solana repositories for the Superteam Security Bounty (Feb 2026).

## Target Repositories

### Tier 1 — Original Targets (audited iterations 1-7)

| # | Repository | Stars | Focus Area |
|---|-----------|-------|------------|
| 1 | [marginfi-v2](https://github.com/mrgnlabs/marginfi-v2) | 284 | Lending — oracle validation, Token-2022 |
| 2 | [anchor](https://github.com/coral-xyz/anchor) | 4,949 | Framework-level, CPI validation |
| 3 | [mpl-token-metadata](https://github.com/metaplex-foundation/mpl-token-metadata) | 243 | NFT state machine |
| 4 | [jito-solana](https://github.com/jito-foundation/jito-solana) | 681 | Validator fork, targeted MEV scan |

### Tier 2 — Expanded Targets (high-value DeFi/infra)

| # | Repository | Stars | Focus Area |
|---|-----------|-------|------------|
| 5 | [wormhole](https://github.com/wormhole-foundation/wormhole) | 1,875 | Cross-chain bridge — Solana contracts |
| 6 | [agave](https://github.com/anza-xyz/agave) | 1,669 | Solana validator client |
| 7 | [pinocchio](https://github.com/anza-xyz/pinocchio) | 849 | Zero-copy Solana program framework |
| 8 | [whirlpools](https://github.com/orca-so/whirlpools) | 509 | Concentrated liquidity AMM |
| 9 | [protocol-v2](https://github.com/drift-labs/protocol-v2) | 376 | Perpetuals DEX |
| 10 | [raydium-clmm](https://github.com/raydium-io/raydium-clmm) | 368 | Concentrated liquidity AMM |

### Tier 3 — Additional Targets

| # | Repository | Stars | Focus Area |
|---|-----------|-------|------------|
| 11 | [phoenix-v1](https://github.com/Ellipsis-Labs/phoenix-v1) | 246 | On-chain order book |
| 12 | [pyth-crosschain](https://github.com/pyth-network/pyth-crosschain) | 226 | Oracle — crosschain programs |
| 13 | [squads-v4](https://github.com/Squads-Protocol/v4) | 171 | Multisig protocol |
| 14 | [klend](https://github.com/Kamino-Finance/klend) | 158 | Lending protocol |

> **Eliminated**: solana-program-library (archived), openbook-v2 (stale ~2 years)

## Audit Methodology

7-iteration vulnerability scan across all repos:

1. **Account Validation & Ownership** — UncheckedAccount, missing owner checks
2. **Signer & Authority** — Missing is_signer, unprotected admin functions
3. **PDA Derivation & Seeds** — Non-canonical bumps, seed mismatches
4. **Arithmetic & Math Safety** — Unchecked ops, precision loss, rounding errors
5. **Logic Flows & State Machine** — Reentrancy, check-effect-interaction violations
6. **CPI & Cross-Program** — Arbitrary CPI, unvalidated program IDs
7. **Edge Cases & Integration** — Oracle staleness, dust, account revival

## Scoring

```
TOTAL = SEVERITY(1-5) x POPULARITY(1-5) x EXPLOITABILITY(1-5) x PROOF_QUALITY(1-3) x FIX_SIMPLICITY(1-3)
```

Max score: 1,125. See [SCOREBOARD.md](./SCOREBOARD.md) for ranked findings.

## Structure

```
solana-security-audit/
  repos/               # Cloned forks (14 repos)
  writeup/             # Vulnerability write-ups
  SCOREBOARD.md        # Ranked findings
  STRATEGY.md          # Audit strategy & priority
  bounty-analysis.md   # Bounty requirements & notes
```

## License

MIT
