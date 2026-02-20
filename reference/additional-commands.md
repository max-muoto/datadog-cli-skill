# Additional Datadog Pup CLI Commands

These are less commonly used `pup` commands. For core commands (auth, test, monitors, metrics, logs, dashboards, SLOs, incidents, traces, infrastructure, tags, events), see the main SKILL.md.

## Synthetics

Manage synthetic monitoring.

### `pup synthetics tests list`

List synthetic tests.

### `pup synthetics locations list`

List available synthetics locations.

## Security

Manage security monitoring.

### `pup security rules list`

List security monitoring rules.

### `pup security signals list`

List security signals.

### `pup security findings search`

Search security findings.

## Users

Manage users and roles.

### `pup users list`

List users.

### `pup users get <user-id>`

Get user details.

### `pup users roles list`

List roles.

## Organizations

Manage organizations.

### `pup organizations list`

List organizations.

### `pup organizations get`

Get current organization details.

## API Keys

Manage API keys.

### `pup api-keys list`

List API keys.

### `pup api-keys get <key-id>`

Get API key details.

### `pup api-keys create`

Create an API key.

**Flags:**
- `--name <value> - Key name (required)`

**Examples:**

```bash
pup api-keys create --name="Production Key"
```

### `pup api-keys delete <key-id>`

Delete an API key.

**Flags:**
- `--yes - Skip confirmation prompt`

## RUM

Query Real User Monitoring data.

### `pup rum apps list`

List RUM applications.

### `pup rum sessions list`

List RUM sessions.

### `pup rum metrics list`

List RUM metrics.

### `pup rum metrics get <metric-id>`

Get RUM metric details.

### `pup rum retention-filters list`

List RUM retention filters.

### `pup rum retention-filters get <filter-id>`

Get RUM retention filter details.

## CI/CD

View CI/CD pipeline visibility.

### `pup cicd pipelines list`

List CI/CD pipelines.

### `pup cicd pipelines get`

Get pipeline details.

**Flags:**
- `--pipeline-id <value> - Pipeline ID`

### `pup cicd events search`

Search CI/CD events.

**Flags:**
- `--query <value> - Search query`
- `--from <value> - Start time`

**Examples:**

```bash
pup cicd events search --query="@ci.status:error" --from="1h"
```

### `pup cicd events aggregate`

Aggregate CI/CD events.

**Flags:**
- `--query <value> - Search query`
- `--compute <value> - Metric to compute`
- `--group-by <value> - Field to group by`

**Examples:**

```bash
pup cicd events aggregate --query="*" --compute="count" --group-by="@ci.status"
```

## Downtime

Manage monitor downtime.

### `pup downtime list`

List downtimes.

### `pup downtime get <downtime-id>`

Get downtime details.

### `pup downtime cancel <downtime-id>`

Cancel a downtime.

**Flags:**
- `--yes - Skip confirmation prompt`

## Cases

Manage case management.

### `pup cases create`

Create a case.

### `pup cases search`

Search cases.

### `pup cases get <case-id>`

Get case details.

### `pup cases assign <case-id>`

Assign a case.

### `pup cases archive <case-id>`

Archive a case.

### `pup cases projects`

List case projects.

## On-Call

Manage on-call teams and schedules.

### `pup on-call teams list`

List on-call teams.

### `pup on-call teams create`

Create an on-call team.

### `pup on-call teams get <team-id>`

Get on-call team details.

### `pup on-call teams update <team-id>`

Update an on-call team.

### `pup on-call teams delete <team-id>`

Delete an on-call team.

**Flags:**
- `--yes - Skip confirmation prompt`

### `pup on-call teams memberships`

Manage team memberships.

## Notebooks

Manage investigation notebooks.

### `pup notebooks list`

List notebooks.

### `pup notebooks get <notebook-id>`

Get notebook details.

### `pup notebooks delete <notebook-id>`

Delete a notebook.

**Flags:**
- `--yes - Skip confirmation prompt`

## Error Tracking

Manage error tracking issues.

### `pup error-tracking issues search`

Search error issues.

### `pup error-tracking issues get <issue-id>`

Get error issue details.

## Scorecards

View service quality scores.

### `pup scorecards list`

List scorecards.

### `pup scorecards get <scorecard-id>`

Get scorecard details.

## Service Catalog

Manage service registry.

### `pup service-catalog list`

List services.

### `pup service-catalog get <service-id>`

Get service details.

## Audit Logs

List and search audit logs.

### `pup audit-logs list`

List audit logs.

### `pup audit-logs search`

Search audit logs.

**Flags:**
- `--query <value> - Search query`

## Cost

Track Datadog costs.

### `pup cost projected`

View projected costs.

### `pup cost attribution`

View cost attribution.

### `pup cost by-org`

View cost by organization.

## Usage

View usage and billing metrics.

### `pup usage summary`

View usage summary.

### `pup usage hourly`

View hourly usage.

## App Keys

Manage app key registrations for Action Connections.

### `pup app-keys list`

List registered app keys.

### `pup app-keys get <app-key-id>`

Get app key registration details.

### `pup app-keys register <app-key-id>`

Register an application key.

### `pup app-keys unregister <app-key-id>`

Unregister an application key.

## Investigations

Manage Bits AI investigations for automated root cause analysis.

### `pup investigations trigger`

Trigger a new investigation.

**Flags:**
- `--type <value> - Investigation type: monitor_alert or general`
- `--monitor-id <value> - Monitor ID (for monitor_alert type)`
- `--event-id <value> - Event ID (for monitor_alert type)`
- `--event-ts <value> - Event timestamp in ms (for monitor_alert type)`
- `--tags <value> - Tags (for general type)`
- `--description <value> - Description (for general type)`

**Examples:**

```bash
# From a monitor alert
pup investigations trigger --type=monitor_alert --monitor-id=123456 --event-id="evt-abc" --event-ts=1706918956000

# General investigation
pup investigations trigger --type=general --tags="service:web-store" --description="High error rate"
```

### `pup investigations get <investigation-id>`

Get investigation details.

### `pup investigations list`

List investigations.

**Flags:**
- `--page-limit <value> - Results per page (default: 20)`

## Vulnerabilities

Search and list security vulnerabilities.

### `pup vulnerabilities list`

List vulnerabilities.

**Flags:**
- `--severity <value> - Filter by severity (comma-separated, e.g., "critical,high")`
- `--status <value> - Filter by status (e.g., "open")`

**Examples:**

```bash
pup vulnerabilities list --severity="critical,high" --status="open"
```

### `pup vulnerabilities search`

Search vulnerabilities.

**Flags:**
- `--query <value> - Search query`

**Examples:**

```bash
pup vulnerabilities search --query="severity:critical"
```

## Static Analysis

Manage static analysis for code security and quality.

### `pup static-analysis custom-rulesets list`

List custom security rulesets.

### `pup static-analysis custom-rulesets get <ruleset-id>`

Get custom ruleset details.

### `pup static-analysis sca`

Software Composition Analysis.

### `pup static-analysis coverage`

Code coverage analysis.

### `pup static-analysis ast`

AST analysis.

## Cloud Integrations

Manage cloud provider integrations.

### `pup cloud aws list`

List AWS integrations.

### `pup cloud azure list`

List Azure integrations.

## Integrations

Manage third-party integrations.

### `pup integrations slack list`

List Slack integrations.

### `pup integrations pagerduty list`

List PagerDuty integrations.

## Network

Manage network monitoring.

### `pup network flows list`

Query network flows.

### `pup network devices list`

List network devices.

## Obs Pipelines

Manage observability pipelines.

### `pup obs-pipelines list`

List observability pipelines.

### `pup obs-pipelines get <pipeline-id>`

Get pipeline details.

## Data Governance

Manage sensitive data scanning.

### `pup data-governance scanner rules list`

List scanning rules.

### `pup data-governance scanner rules get <rule-id>`

Get scanning rule details.

## Product Analytics

### `pup product-analytics events send`

Send a product analytics event.

**Flags:**
- `--app-id <value> - Application ID`
- `--event <value> - Event name`
- `--properties <value> - JSON properties`
- `--user-id <value> - User ID`

**Examples:**

```bash
pup product-analytics events send \
  --app-id=my-app \
  --event=purchase_completed \
  --properties='{"amount":99.99}' \
  --user-id=user-123
```

## Miscellaneous

### `pup misc ip-ranges`

Get Datadog IP ranges.

### `pup misc status`

Check Datadog service status.
