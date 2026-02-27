# IEX Cloud API Reference (Operational Summary)

Primary references:
- https://iexcloud.io/docs/api/
- https://status.iexapis.com/

## Current Access Note (Date-Sensitive)

As of February 26, 2026:
- `status.iexapis.com` reports API components as operational.
- public docs/site availability on `iexcloud.io` can be inconsistent from some networks.

Treat endpoint behavior and status page as source of truth when docs are temporarily unavailable.

## Base URLs and Auth

- Production base: `https://cloud.iexapis.com/stable`
- Sandbox base: `https://sandbox.iexapis.com/stable`
- Authentication: append `token=YOUR_TOKEN` query parameter

Example pattern:

```text
GET https://cloud.iexapis.com/stable/stock/AAPL/quote?token=YOUR_TOKEN
```

## Common Endpoints

## 1) Single-Symbol Market Data

- Quote: `/stock/{symbol}/quote`
- Company: `/stock/{symbol}/company`
- Stats: `/stock/{symbol}/stats`
- News: `/stock/{symbol}/news/last/{last}`
- Chart: `/stock/{symbol}/chart/{range}`

Typical chart ranges include: `1d`, `5d`, `1m`, `3m`, `6m`, `ytd`, `1y`, `2y`, `5y`, `max`.

## 2) Market Lists

- `/stock/market/list/{listType}`

Common list types:
- `mostactive`
- `gainers`
- `losers`
- `iexvolume`
- `iexpercent`

## 3) Batch

- `/stock/market/batch`

Required query params:
- `symbols` (comma-separated)
- `types` (comma-separated, such as `quote`, `stats`, `company`, `chart`)

Example:

```text
GET /stock/market/batch?symbols=AAPL,MSFT&types=quote,stats&token=...
```

## Response and Error Guardrails

Every integration should handle:

1. Network failures and timeouts.
2. Non-2xx HTTP responses (invalid token, bad params, quota/plan limits).
3. JSON payloads carrying API-level error messages.
4. Missing keys / empty arrays for thinly traded symbols or unsupported data.

Operational recommendation:
- Fail fast on transport and HTTP errors.
- Validate required fields before downstream calculations.
- Preserve request path and params (excluding secrets) for reproducibility.

## CLI Mapping in This Skill

`scripts/iex_cloud_cli.sh` supports:
- `quote SYMBOL`
- `chart SYMBOL RANGE`
- `company SYMBOL`
- `stats SYMBOL`
- `news SYMBOL [LAST]`
- `movers LIST_TYPE`
- `batch SYMBOLS TYPES`
- `raw PATH [key=value ...]`

Environment variables:
- `IEX_TOKEN` or `IEX_CLOUD_TOKEN`
- optional `IEX_BASE_URL`
