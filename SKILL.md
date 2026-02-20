---
name: datadog-cli
description: Guide for using the Datadog pup CLI to interact with Datadog from the command line. Use when the user asks about viewing monitors, dashboards, metrics, logs, SLOs, incidents, traces, infrastructure, or authenticating with Datadog via CLI.
---

# Datadog Pup CLI Usage Guide

Help users interact with Datadog from the command line using the `pup` CLI.

## Prerequisites

The CLI must be installed and authenticated before use.

### Installation

```bash
# Homebrew
brew tap datadog/pack
brew install datadog/pack/pup

# Or install via Go
go install github.com/DataDog/pup@latest
```

### Authentication

```bash
# OAuth2 login via browser (recommended)
pup auth login

# Using API keys (set environment variables)
export DD_API_KEY="your-api-key"
export DD_APP_KEY="your-app-key"
export DD_SITE="datadoghq.com"

# Check auth status
pup auth status

# Refresh expired token
pup auth refresh

# Logout
pup auth logout
```

### Multi-Site Support

```bash
DD_SITE=datadoghq.com pup auth login      # US1 (default)
DD_SITE=datadoghq.eu pup auth login       # EU1
DD_SITE=us3.datadoghq.com pup auth login  # US3
DD_SITE=us5.datadoghq.com pup auth login  # US5
DD_SITE=ap1.datadoghq.com pup auth login  # AP1
```

## Global Flags

All commands support these flags:

- `--output, -o <value> - Output format: json, table, yaml (default: json)`
- `--yes, -y - Skip confirmation prompts for destructive operations`
- `--site <value> - Datadog site (default: datadoghq.com)`
- `--verbose - Enable verbose logging`

## Available Commands

### Auth

Authenticate with Datadog.

#### `pup auth login`

Authenticate via OAuth2 browser flow or API keys.

**Examples:**

```bash
pup auth login
```

#### `pup auth status`

View authentication status.

**Examples:**

```bash
pup auth status
```

#### `pup auth refresh`

Refresh your OAuth2 access token.

**Examples:**

```bash
pup auth refresh
```

#### `pup auth logout`

Log out and clear stored tokens.

**Examples:**

```bash
pup auth logout
```

### Test

#### `pup test`

Test connection and display configuration info.

**Examples:**

```bash
pup test
```

### Monitors

Manage Datadog monitors for alerting and notifications.

#### `pup monitors list`

List monitors.

**Flags:**
- `--name <value> - Filter by monitor name`
- `--tags <value> - Filter by tags (comma-separated, e.g., "env:prod,team:backend")`
- `--limit <value> - Maximum monitors to return (default: 200, max: 1000)`

**Examples:**

```bash
pup monitors list

pup monitors list --name="CPU"

pup monitors list --tags="env:production"

pup monitors list --limit=500
```

#### `pup monitors get <monitor-id>`

Get details of a specific monitor.

**Examples:**

```bash
pup monitors get 12345678

pup monitors get 12345678 --output=table
```

#### `pup monitors delete <monitor-id>`

Delete a monitor.

**Flags:**
- `--yes - Skip confirmation prompt`

**Examples:**

```bash
pup monitors delete 12345678

pup monitors delete 12345678 --yes
```

#### `pup monitors search`

Search monitors.

**Flags:**
- `--query <value> - Search query string`
- `--page <value> - Page number (default: 0)`
- `--per-page <value> - Results per page (default: 30)`
- `--sort <value> - Sort order (e.g., "name,asc", "id,desc")`

**Examples:**

```bash
pup monitors search --query="database"

pup monitors search --query="cpu" --page=1 --per-page=50
```

### Metrics

Query time-series metrics and manage metric metadata.

#### `pup metrics query`

Query metrics using the v2 API.

**Flags:**
- `--query <value> - Metric query string (required)`
- `--from <value> - Start time (default: "1h")`
- `--to <value> - End time (default: "now")`

Query syntax: `<aggregation>:<metric_name>{<filter>} [by {<group>}]`

**Examples:**

```bash
pup metrics query --query="avg:system.cpu.user{*}" --from="1h"

pup metrics query --query="sum:app.requests{env:prod} by {service}" --from="4h"

pup metrics query --query="max:system.disk.used{host:web-*}" --from="2h"
```

#### `pup metrics search`

Search metrics using the v1 API.

**Flags:**
- `--query <value> - Metric query string (required)`
- `--from <value> - Start time (default: "1h")`
- `--to <value> - End time (default: "now")`

**Examples:**

```bash
pup metrics search --query="avg:system.cpu.user{*}" --from="1h"
```

#### `pup metrics list`

List available metrics.

**Flags:**
- `--filter <value> - Filter by metric name pattern (e.g., "system.*")`
- `--tag-filter <value> - Filter by tags (e.g., "env:prod,service:api")`

**Examples:**

```bash
pup metrics list

pup metrics list --filter="system.*"

pup metrics list --tag-filter="env:prod"
```

#### `pup metrics metadata get <metric-name>`

Get metadata for a metric.

**Examples:**

```bash
pup metrics metadata get system.cpu.user
```

#### `pup metrics metadata update <metric-name>`

Update metadata for a metric.

**Flags:**
- `--description <value> - Metric description`
- `--unit <value> - Unit of measurement`
- `--type <value> - Metric type (gauge, count, rate, distribution)`
- `--per-unit <value> - Per-unit for rate metrics`
- `--short-name <value> - Short display name`

**Examples:**

```bash
pup metrics metadata update system.cpu.user \
  --description="CPU user time" \
  --unit="percent" \
  --type="gauge"
```

#### `pup metrics submit`

Submit custom metrics.

**Flags:**
- `--name <value> - Metric name (required)`
- `--value <value> - Metric value (required)`
- `--type <value> - Metric type: gauge, count, rate (default: "gauge")`
- `--timestamp <value> - Unix timestamp or "now" (default: "now")`
- `--tags <value> - Comma-separated tags (e.g., "env:prod,team:api")`
- `--host <value> - Host name`
- `--interval <value> - Interval in seconds for rate/count metrics`

**Examples:**

```bash
pup metrics submit --name="custom.temperature" --value=72.5

pup metrics submit \
  --name="custom.api.requests" \
  --value=1250 \
  --tags="env:prod,service:api,region:us-east-1"
```

#### `pup metrics tags list <metric-name>`

List tags for a metric.

**Flags:**
- `--from <value> - Start time (default: "1h")`
- `--to <value> - End time (default: "now")`

**Examples:**

```bash
pup metrics tags list system.cpu.user
```

### Logs

Search and analyze log data.

#### `pup logs search`

Search logs using v1 API.

**Flags:**
- `--query <value> - Log search query (required)`
- `--from <value> - Start time (required)`
- `--to <value> - End time (default: "now")`
- `--limit <value> - Max logs to return (default: 50, max: 1000)`
- `--sort <value> - Sort order: asc or desc (default: desc)`
- `--index <value> - Comma-separated log indexes`
- `--storage <value> - Storage tier: indexes, online-archives, or flex`

**Examples:**

```bash
pup logs search --query="status:error" --from="1h"

pup logs search --query="service:api" --from="2h" --to="1h"

pup logs search --query="@http.status_code:500" --from="30m" --limit=100

pup logs search --query="status:error" --from="1h" --storage="flex"
```

#### `pup logs list`

List logs using v2 API.

**Flags:**
- `--query <value> - Log search query (default: "*")`
- `--from <value> - Start time (required)`
- `--to <value> - End time (default: "now")`
- `--limit <value> - Number of logs (default: 10)`
- `--sort <value> - Sort order: timestamp, -timestamp (default: -timestamp)`
- `--storage <value> - Storage tier: indexes, online-archives, or flex`

**Examples:**

```bash
pup logs list --from="1h"

pup logs list --query="service:api" --from="2h" --limit=50
```

#### `pup logs query`

Query logs using v2 API.

**Flags:**
- `--query <value> - Log query (required)`
- `--from <value> - Start time (required)`
- `--to <value> - End time (default: "now")`
- `--limit <value> - Max results (default: 50)`
- `--sort <value> - Sort order: timestamp, -timestamp`
- `--timezone <value> - Timezone for timestamps`
- `--storage <value> - Storage tier: indexes, online-archives, or flex`

**Examples:**

```bash
pup logs query --query="status:error" --from="1h"

pup logs query --query="service:web" --from="4h" --timezone="America/New_York"
```

#### `pup logs aggregate`

Aggregate log data.

**Flags:**
- `--query <value> - Log query (required)`
- `--from <value> - Start time (required)`
- `--to <value> - End time (default: "now")`
- `--compute <value> - Metric to compute (default: "count")`
- `--group-by <value> - Field to group by`
- `--limit <value> - Max groups (default: 10)`
- `--storage <value> - Storage tier: indexes, online-archives, or flex`

Compute metrics: count, cardinality, avg, sum, min, max, percentile.

**Examples:**

```bash
pup logs aggregate --query="*" --from="1h" --compute="count" --group-by="status"

pup logs aggregate --query="service:api" --from="2h" --compute="percentile(@duration, 99)"
```

#### `pup logs archives list`

List log archives.

#### `pup logs archives get <archive-id>`

Get log archive details.

#### `pup logs archives delete <archive-id>`

Delete a log archive.

**Flags:**
- `--yes - Skip confirmation prompt`

#### `pup logs metrics list`

List log-based metrics.

#### `pup logs metrics get <metric-id>`

Get log-based metric details.

#### `pup logs metrics delete <metric-id>`

Delete a log-based metric.

**Flags:**
- `--yes - Skip confirmation prompt`

#### `pup logs custom-destinations list`

List custom log destinations.

#### `pup logs custom-destinations get <destination-id>`

Get custom log destination details.

### Dashboards

Manage Datadog dashboards.

#### `pup dashboards list`

List dashboards.

**Examples:**

```bash
pup dashboards list

pup dashboards list --output=table

pup dashboards list | jq '.dashboards[] | select(.title | contains("API"))'
```

#### `pup dashboards get <dashboard-id>`

Get dashboard details.

**Examples:**

```bash
pup dashboards get abc-def-123

# Export dashboard to file
pup dashboards get abc-def-123 > dashboard.json

# Inspect widgets
pup dashboards get abc-def-123 | jq '.widgets'
```

#### `pup dashboards delete <dashboard-id>`

Delete a dashboard.

**Flags:**
- `--yes - Skip confirmation prompt`

**Examples:**

```bash
pup dashboards delete abc-def-123 --yes
```

#### `pup dashboards url <dashboard-id>`

Get the URL for a dashboard.

### SLOs

Manage Service Level Objectives.

#### `pup slos list`

List SLOs.

**Examples:**

```bash
pup slos list

# Find breaching SLOs
pup slos list | jq '.data[] | select(.status.state == "breaching")'
```

#### `pup slos get <slo-id>`

Get SLO details.

**Examples:**

```bash
pup slos get abc-123-def

pup slos get abc-123-def | jq '.data.error_budget_remaining'
```

#### `pup slos delete <slo-id>`

Delete an SLO.

**Flags:**
- `--yes - Skip confirmation prompt`

#### `pup slos corrections`

Manage SLO corrections.

### Incidents

Manage incidents.

#### `pup incidents list`

List incidents.

**Examples:**

```bash
pup incidents list

# Filter active SEV-1 incidents
pup incidents list | jq '.data[] | select(.severity == "SEV-1")'
```

#### `pup incidents get <incident-id>`

Get incident details.

**Examples:**

```bash
pup incidents get abc-123-def

pup incidents get abc-123-def | jq '.data.timeline'
```

#### `pup incidents attachments <incident-id>`

Get incident attachments.

### Infrastructure

Manage hosts and infrastructure.

#### `pup infrastructure hosts list`

List hosts.

#### `pup infrastructure hosts get <hostname>`

Get host details.

### Tags

Manage host tags.

#### `pup tags list`

List all tags.

#### `pup tags get <hostname>`

Get tags for a host.

#### `pup tags add <hostname> <tag1> <tag2> ...`

Add tags to a host.

**Examples:**

```bash
pup tags add my-host env:prod team:backend
```

#### `pup tags update <hostname> <tag1> <tag2> ...`

Update all tags on a host.

#### `pup tags delete <hostname>`

Delete all tags from a host.

### Events

Manage events.

#### `pup events list`

List events.

#### `pup events search`

Search events.

**Flags:**
- `--query <value> - Search query`

#### `pup events get <event-id>`

Get event details.

### Traces

Query APM traces and spans for distributed tracing analysis.

#### `pup traces`

Query APM traces.

**Examples:**

```bash
pup traces
```

For additional commands (synthetics, security, users, organizations, RUM, CI/CD, downtime, cases, on-call, notebooks, error-tracking, scorecards, service-catalog, audit-logs, cost, usage, app-keys, investigations, vulnerabilities, static-analysis, cloud, integrations, network, obs-pipelines, data-governance, product-analytics, misc), see reference/additional-commands.md.

## Time Range Formats

Supported across search and query commands:

- **Relative:** `1h`, `30m`, `7d`, `1w`, `5min`, `2hours`, `3days`
- **Absolute:** Unix timestamp (`1704067200`), RFC3339 (`2024-01-01T00:00:00Z`)
- **Special:** `now`

## Output Formats

### JSON Output (default)

```bash
pup monitors list | jq '.[] | .name'
```

### Table Output

```bash
pup monitors list --output=table
```

### YAML Output

```bash
pup monitors list --output=yaml
```

## Environment Variables

- `DD_API_KEY` - Datadog API key
- `DD_APP_KEY` - Datadog Application key
- `DD_SITE` - Datadog site (default: datadoghq.com)
- `DD_AUTO_APPROVE` - Auto-approve destructive operations (true/false)
- `DD_TOKEN_STORAGE` - Token storage backend: keychain or file
