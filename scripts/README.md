# IEX Cloud CLI

Local helper for common IEX Cloud REST calls.

## Prerequisites

- `curl` required
- `jq` optional (for pretty JSON)
- token in `IEX_TOKEN` or `IEX_CLOUD_TOKEN`

## Usage

```bash
chmod +x scripts/iex_cloud_cli.sh
scripts/iex_cloud_cli.sh --help
```

## Examples

```bash
scripts/iex_cloud_cli.sh quote AAPL
scripts/iex_cloud_cli.sh chart AAPL 3m
scripts/iex_cloud_cli.sh company MSFT
scripts/iex_cloud_cli.sh stats NVDA
scripts/iex_cloud_cli.sh news TSLA 5
scripts/iex_cloud_cli.sh movers mostactive
scripts/iex_cloud_cli.sh batch AAPL,MSFT quote,stats
scripts/iex_cloud_cli.sh raw stock/AAPL/quote
```

Sandbox example:

```bash
scripts/iex_cloud_cli.sh --sandbox quote AAPL
```
