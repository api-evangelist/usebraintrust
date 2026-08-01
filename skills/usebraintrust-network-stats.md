---
name: Read Braintrust network statistics
description: >-
  Pull live Braintrust (usebraintrust.com) talent-network metrics — network
  totals, weekly GSV series, per-skill average hourly rates, and the BTRST
  token-earner leaderboard — from the public Network Stats API.
api: openapi/usebraintrust-network-stats-openapi.yml
operations: [getDashboard, getSkillRates, getTokenEarners]
generated: '2026-07-21'
method: generated
---

# Read Braintrust network statistics

The Braintrust Network Stats API is an unauthenticated, read-only JSON API at
`https://dashboard.app.usebraintrust.com`. It backs the public dashboard at
https://info.app.usebraintrust.com/. It is observed, not provider-documented —
treat it as a best-effort open-data surface.

## Steps

1. **Get the network snapshot** — `getDashboard` (`GET /api/dashboard`).
   Read `data.dashboard` for totals (`total_clients`, `total_jobs`,
   `total_talent`, `lifetime_network_gsv`, `token_circulating_supply`, …) and
   `data.gsv_data[]` for the weekly GSV time series (entries since
   2020-01-06, `{date, value}`).
2. **Get rates by skill** — `getSkillRates`
   (`GET /api/dashboard/skill_rates`). `data.skill_rates[]` maps ~1,100
   skills to `avg_rate` (USD/hour).
3. **Get the token leaderboard** — `getTokenEarners`
   (`GET /api/dashboard/token_earners`). `data.token_earners[]` lists public
   community members with referral counts and BTRST tokens earned.

## Rules

- No authentication, API key, or OAuth — send plain GETs.
- Monetary and token amounts are **decimal strings**, not numbers — parse
  with a decimal type, never float.
- No pagination: each endpoint returns the full collection in one response.
- Errors are framework-default **HTML** 404s, not JSON — check the
  `Content-Type` before parsing.
- No published rate limits — be polite; the data snapshot updates roughly
  daily (`data.dashboard.created_at`).
