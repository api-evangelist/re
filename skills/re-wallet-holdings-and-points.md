---
name: Look up a wallet's Re holdings and points
description: >-
  Retrieve a wallet address's reUSD/reUSDe balances across chains and its Re
  Points total and leaderboard rank from the public Re Protocol API.
api: openapi/re-openapi.yml
operations: [getWalletBalance, getWalletPoints, getPointsLeaderboard]
---

# Look up a wallet's Re holdings and points

The Re Protocol API (`https://api.re.xyz`) is public and read-only. The wallet
endpoints require an `address` query parameter.

## Steps

1. **Balances.** `GET /wallet/balance?address=0x...` (operation
   `getWalletBalance`). Optional `token` param (reUSD/reUSDe, default both)
   returns holdings across chains.
2. **Points and rank.** `GET /wallet/points?address=0x...` (operation
   `getWalletPoints`) returns accumulated Re Points and leaderboard position.
3. **Context.** `GET /points/public/leaderboard?limit=100&offset=0` (operation
   `getPointsLeaderboard`) to place the wallet against the public leaderboard;
   `season` defaults to the current season.

## Rules

- `address` is required on both wallet operations — omitting it returns a 400
  (see `errors/re-problem-types.yml`).
- The leaderboard is limit/offset paginated (`limit` 1-100, default 100).
- Check `success === true` before reading the payload.
