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
6. **Repo must NOT be archived or stale** (upstream PR required, active development)

## Eliminated Findings (Archived/Stale Repos)

**solana-program-library** (solana-labs) — **archived** March 2025:
- ~~Missing Pyth oracle confidence interval check (score 720)~~
- ~~Flash loan host_fee_receiver unvalidated (score 360)~~
- ~~Liquidation rounding asymmetry (score 120)~~
- ~~Flash loan @FIXME u64::MAX fee (score 120)~~
- ~~Compound interest precision loss (score 40)~~

**openbook-v2** (openbook-dex) — **stale** last commit June 2024 (~2 years):
- ~~Unchecked i64 arithmetic in order matching (score 144)~~

## Active Findings — Full Sweep (14 repos audited)

| # | Repo | Vulnerability | Sev | Pop | Expl | Proof | Fix | **Total** | Status |
|---|------|--------------|-----|-----|------|-------|-----|-----------|--------|
| F2 | **Anchor** | CPI Return<T> unvalidated program ID | 4 | 5 | 3 | 3 | 3 | **540** | **SUBMITTED** |
| F5 | **Phoenix** | Ask-side fee underflow in fifo.rs:932 | 4 | 2 | 3 | 3 | 3 | **216** | Verified |
| F6 | **Phoenix** | Quote lot validation overflow in new_order.rs:754 | 3 | 2 | 3 | 3 | 3 | **162** | Verified |
| F10 | **Agave** | Vote state increment_credits unwrap panic | 3 | 4 | 2 | 2 | 3 | **144** | DoS only |
| F7 | **Pinocchio** | Unbounded align_offset in lazy entrypoint | 3 | 3 | 2 | 2 | 3 | **108** | Defensive coding |
| F8 | **Pinocchio** | Buffer underflow in Instructions sysvar | 3 | 3 | 2 | 2 | 3 | **108** | unchecked fn |
| F9 | **Pinocchio** | Unvalidated offset in Instructions deser | 3 | 3 | 2 | 2 | 3 | **108** | unchecked fn |
| F3 | **Raydium** | Fee rate sum validation bypass (admin-only DoS) | 3 | 2 | 2 | 3 | 3 | **108** | Admin-gated |
| F4 | **Raydium** | Trade fee rate underflow (admin-only) | 3 | 2 | 2 | 2 | 3 | **72** | Admin-gated |
| F11 | **Phoenix** | Multi-order balance tracking underflow | 3 | 2 | 2 | 2 | 3 | **72** | Edge case |
| F12 | **Drift** | Confidence interval inflation via MM oracle | 4 | 2 | 2 | 2 | 2 | **64** | Design issue |
| F13 | **Drift** | Unsafe unwrap in Pyth oracle deserialization | 2 | 2 | 2 | 2 | 3 | **48** | DoS only |
| F14 | **Klend** | Division by Zero in LTV public methods | 3 | 1 | 2 | 2 | 3 | **36** | Low impact |

### False Positives (Full Sweep)

| Repo | Finding | Reason |
|------|---------|--------|
| MarginFi v2 | StakeStateV2 owner check | Proper checks exist in account validation |
| MarginFi v2 | Token-2022 transfer hook gap | Protocol restricts to known mints |
| MarginFi v2 | Switchboard oracle staleness | Handled via max_staleness config |
| Pyth | Unrestricted update_price_feed in push-oracle | By design — permissionless relay, integrity from Wormhole guardian sigs |
| Wormhole | Decimal overflow (transfer.rs:221) | Only ≥28 decimals, no real token uses this |
| Wormhole | Off-by-one (verify_signature.rs:199) | Self-DoS only, no economic impact |
| Klend | Interest approximation precision | Named "approximate", intentional |
| Klend | Pyth confidence overflow | Bounded by realistic values |
| Whirlpools | Fee growth wrapping | By design (Uniswap v3 pattern) |
| Whirlpools | Protocol fee wrapping | By design, documented |
| Pinocchio | Unvalidated instruction_data_len | SVM runtime enforces limits |

### Clean Repos (0 exploitable findings)

| Repo | Stars | Notes |
|------|-------|-------|
| Squads v4 | 171 | Pristine — excellent invariant checking |
| Metaplex | 243 | Pristine — comprehensive validation |
| Whirlpools | 509 | By-design wrapping only |
| Jito | 681 | Clean — 2 minor operational issues |
| Wormhole | 1,875 | 2 informational only |

## Selected Target

**Anchor F2 — CPI Return<T> unvalidated program ID** (Score: 540)
- Active repo (pushed Jan 25, 2026) — 4,949 stars
- Framework-level impact — affects ALL Anchor programs using CPI return data
- `cpi.rs:93-94` discards `_key` from `get_return_data()` (the program ID)
- Same repo's `token_2022.rs:377-382` implements the **correct pattern** (validates program ID)
- Severity upgraded: attacker can spoof return data via nested CPI

## Runner-Up: Phoenix F5 (Score: 216)

**Phoenix — Ask-side fee underflow** (fifo.rs:932-936)
- Unchecked subtraction: `round_down(matched) / B - quote_lot_fees` can underflow
- When `matched_adjusted < base_lots_per_base_unit`, floor = 0 but fees round up to ≥1
- Result: u64 wraps to massive value → incorrect fill amounts
- Fix: `saturating_sub()` or `checked_sub()`

## Submission Status

- [x] Anchor F2 — PoC, fix, write-up, PR, API submission complete
- [x] PR: https://github.com/solana-foundation/anchor/pull/4231
- [x] Gist: https://gist.github.com/rz1989s/f0d0d217c27a28f89bbec11b3b439cb6
- [x] Submission ID: 2d8744f3-8888-4810-ac7c-ed8457081de0
- [ ] Consider updating submission to include Phoenix findings as additional PRs
