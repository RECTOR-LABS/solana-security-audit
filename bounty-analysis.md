# Bounty Analysis

## Bounty Details

- **Title**: Audit & Fix Open-Source Solana Repositories for Vulnerabilities
- **Prize**: 3,000 USDG (1st: 1,500 / 2nd: 1,000 / 3rd: 500)
- **Deadline**: Feb 15, 2026 18:29 UTC
- **Submissions**: 14 competing, 3 winners
- **Listing ID**: `4b408d2a-a09e-4584-b0e1-9bd534c23054`

## Requirements

1. Find a security vulnerability in a popular Solana open-source repository
2. Submit a fix as a pull request **to the upstream repo** (must not be archived)
3. Provide a detailed write-up explaining the vulnerability, impact, and fix
4. Write-up should include proof of concept or clear reproduction steps

## Judging Criteria (inferred)

- **Impact**: How severe is the vulnerability? (fund loss > DoS > information leak)
- **Novelty**: Is this a new finding or known issue?
- **Quality**: How thorough is the write-up and fix?
- **Repository popularity**: Higher-profile repos likely score better
- **Fix quality**: Clean, minimal, correct patch

## Target Repositories

Five active Solana repos for audit:
1. marginfi-v2
2. openbook-v2
3. anchor
4. mpl-token-metadata
5. jito-solana

**Eliminated**: solana-program-library — archived (March 2025), cannot create upstream PRs.

## Submission Format

Via Superteam Agent API:
- `link`: PR URL to **upstream** repo (not fork)
- `otherInfo`: Vulnerability description
- `eligibilityAnswers`: `[{question, answer}]` — link to detailed write-up (GitHub Gist)
- `telegram`: `http://t.me/RZ1989sol`

## Competitive Analysis

14 submissions competing for 3 spots. Need to be in top 3.
Differentiators:
- Systematic methodology (7-iteration scan)
- High-quality PoC with reproducible tests
- Professional write-up format
- Upstream PR to active repo
