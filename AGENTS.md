<!-- Satellite context file — extends the global hub (~/.claude/CLAUDE.md | ~/.pi/agent/AGENTS.md). Host-neutral; project-specific only. Do not duplicate hub standards here. -->

# Solana Security Audit

> 🏆 **1st Place Winner — Superteam Security Bounty ($1,500 USDG).** Won 1st place out of 116 submissions in the "Audit & Fix Open-Source Solana Repositories for Vulnerabilities" bounty on Superteam Earn (March 2026). Systematic audit of 14 open-source Solana repositories, identifying 13 real vulnerabilities across 7 repos.

**Submitted finding:** Anchor Framework CPI Return Data Spoofing (CVSSv3 7.5).

## Structure

`SCOREBOARD.md` (audit scoreboard) · `TRACKER.md` (progress tracker) · `STRATEGY.md` (audit strategy) · `bounty-analysis.md` · `bounty-original.md` · `writeup/` + `writeups/` (per-finding writeups) · `repos/` (audited repo refs) · `README.md`.

## Target Repositories

Tier 1 — Original Targets (audited iterations 1-7). See `README.md` and `SCOREBOARD.md` for the full table (14 repos, stars, focus area, findings).

## Notes

- The crown-jewel finding (Anchor CPI return-data spoofing, CVSS 7.5, 1st of 116 across a 14-protocol audit) anchors the `RECTOR-LABS/solana-cpi-safety-skill` Claude Code skill. **Claim accuracy is strict** — see that skill's AGENTS.md "Claim accuracy" section; never inflate the metrics.
- This repo is the audit record + writeups; the skill repo is the deliverable product.