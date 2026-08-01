---
name: Check Re Protocol yields and TVL
description: >-
  Read current reUSD/reUSDe APY and NAV, token supply, and protocol TVL from the
  public Re Protocol API. No authentication required.
api: openapi/re-openapi.yml
operations: [getApy, getSupply, getTvl, getTvlHistory]
---

# Check Re Protocol yields and TVL

The Re Protocol API (`https://api.re.xyz`) is public and read-only — no API key
or auth header is required. All responses use the envelope
`{ success, data|results, timestamp }` with UTC timestamps.

## Steps

1. **Current yields.** `GET /apy` (operation `getApy`). Read
   `data.reUSD.apy` / `data.reUSDe.apy` for annualized yield and
   `data.<token>.currentNAV` for net asset value. Optional `period` (1d-30d /
   1M-12M, default 7d) and `tokens` query params.
2. **Supply.** `GET /supply` (operation `getSupply`). Read
   `results.reUSD.totalSupply` / `results.reUSDe.circulating`. Supplies are
   full-precision decimal strings — parse as decimals, not floats.
3. **Protocol capital.** `GET /tvl` (operation `getTvl`) for the latest value of
   every metric (`total`, `onchain_capital`, `offchain_capital`,
   `premium-receivables`), each as `{ amount, lastUpdated, currency }`.
4. **History.** `GET /tvl/history` (operation `getTvlHistory`) for the daily
   series when charting trends.

## Rules

- Check `success === true` before reading the payload (see
  `errors/re-problem-types.yml`).
- Preserve NAV/supply decimal strings; do not coerce to float.
- No pagination on these endpoints; the leaderboard endpoints use limit/offset
  (see `conventions/re-conventions.yml`).
