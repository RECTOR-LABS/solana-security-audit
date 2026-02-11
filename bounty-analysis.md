# Bounty Analysis

## Bounty Details

- **Title**: Audit & Fix Open-Source Solana Repositories for Vulnerabilities
- **Prize**: 3,000 USDG (1st: 1,500 / 2nd: 1,000 / 3rd: 500)
- **Deadline**: Feb 15, 2026 18:29 UTC
- **Submissions**: 13 competing, 3 winners
- **Listing ID**: `4b408d2a-a09e-4584-b0e1-9bd534c23054`

## Requirements

1. Find a security vulnerability in a popular Solana open-source repository
2. Submit a fix as a pull request to the upstream repo
3. Provide a detailed write-up explaining the vulnerability, impact, and fix
4. Write-up should include proof of concept or clear reproduction steps

## Judging Criteria (inferred)

- **Impact**: How severe is the vulnerability? (fund loss > DoS > information leak)
- **Novelty**: Is this a new finding or known issue?
- **Quality**: How thorough is the write-up and fix?
- **Repository popularity**: Higher-profile repos likely score better
- **Fix quality**: Clean, minimal, correct patch

## Target Repositories

Six popular Solana repos identified for audit:
1. solana-program-library (token-lending focus)
2. openbook-v2
3. marginfi-v2
4. anchor
5. mpl-token-metadata
6. jito-solana

## Submission Format

Via Superteam Agent API:
- `link`: PR URL to upstream repo
- `otherInfo`: Vulnerability description
- `eligibilityAnswers`: Link to detailed write-up (GitHub Gist)

## Competitive Analysis

13 submissions competing for 3 spots. Need to be in top 3.
Differentiators:
- Systematic methodology (7-iteration scan)
- High-quality PoC with reproducible tests
- Professional write-up format
- Targeting unaudited code (SPL token-lending)
