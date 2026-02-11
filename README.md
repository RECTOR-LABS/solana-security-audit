# Solana Security Audit

Systematic security audit of open-source Solana repositories for the Superteam Security Bounty (Feb 2026).

## Target Repositories

| Priority | Repository | Focus Area |
|----------|-----------|------------|
| 1 | [marginfi-v2](https://github.com/mrgnlabs/marginfi-v2) | Lending — oracle validation, Token-2022 |
| 2 | [openbook-v2](https://github.com/openbook-dex/openbook-v2) | DEX orderbook, i64 arithmetic |
| 3 | [anchor](https://github.com/coral-xyz/anchor) | Framework-level, CPI validation |
| 4 | [mpl-token-metadata](https://github.com/metaplex-foundation/mpl-token-metadata) | NFT state machine |
| 5 | [jito-solana](https://github.com/jito-foundation/jito-solana) | Validator fork, targeted MEV scan |

> **Eliminated**: solana-program-library — archived March 2025, cannot create upstream PRs.

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
  repos/               # Cloned forks
  writeup/             # Vulnerability write-ups
  SCOREBOARD.md        # Ranked findings
  STRATEGY.md          # Audit strategy & priority
  bounty-analysis.md   # Bounty requirements & notes
```

## License

MIT
