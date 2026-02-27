---
name: iex-cloud
description: Use this skill when a task needs IEX Cloud market data through the REST API (quotes, charts, fundamentals, market lists, and batch calls), including secure token handling and scriptable CLI usage.
---

# IEX Cloud

## Overview

This skill provides an operational workflow for IEX Cloud API usage in OpenClaw tasks:
- selecting the right endpoint for market-data requests
- building valid authenticated requests
- handling API and transport errors
- running repeatable calls through a local Bash CLI

## Quick Start

1. Set token:
- `export IEX_TOKEN=...` (recommended)
- or `export IEX_CLOUD_TOKEN=...`
2. Read endpoint/parameter guidance in `references/api_docs.md`.
3. Use `scripts/iex_cloud_cli.sh` for reliable calls.

Example:

```bash
scripts/iex_cloud_cli.sh quote AAPL
scripts/iex_cloud_cli.sh chart AAPL 1m
scripts/iex_cloud_cli.sh movers mostactive
```

## Workflow

1. Classify request type:
- latest quote: `quote`
- historical bars: `chart`
- company/fundamentals: `company`, `stats`
- market movers: `movers`
- multi-symbol pulls: `batch`
2. Validate required parameters before call dispatch.
3. Execute request with token auth and timeout.
4. Validate response class:
- HTTP failure / transport failure
- JSON payload containing API error fields
- empty or malformed payload
5. Normalize output downstream as needed.

## Authentication and Safety

- Default token env vars: `IEX_TOKEN`, `IEX_CLOUD_TOKEN`.
- Do not hardcode tokens in source files.
- Do not print full token values in logs.
- Prefer query parameter `token=...` when using these endpoints.

## Reliability Guidance

- Use bounded timeouts (`curl --max-time` in CLI).
- Handle non-2xx responses as hard failures.
- Validate symbol, range, and list-type inputs early.
- For large jobs, use batch endpoints where possible.

## Included Files

- `scripts/iex_cloud_cli.sh`: Bash CLI for common endpoints and raw calls.
- `scripts/README.md`: CLI usage examples and command reference.
- `references/api_docs.md`: operational endpoint reference and guardrails.

## Resources

- API docs: https://iexcloud.io/docs/api/
- Status page: https://status.iexapis.com/
- Base URL (stable): `https://cloud.iexapis.com/stable`
- Sandbox URL: `https://sandbox.iexapis.com/stable`
