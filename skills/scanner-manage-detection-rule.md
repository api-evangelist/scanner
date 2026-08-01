---
name: Create and manage a Scanner detection rule
description: Create, list, update, and delete streaming detection rules that alert on matching log events.
api: openapi/scanner-openapi.yml
operations: [createDetectionRule, listDetectionRules, getDetectionRule, updateDetectionRule, deleteDetectionRule]
---

# Create and manage a Scanner detection rule

Use this to stand up a continuous detection that runs a query on a schedule and routes alerts to event sinks.

## Auth
`Authorization: Bearer <Scanner API Key>`; include `tenant_id`.

## Steps
1. Ensure you have `event_sink_ids` to route alerts (see the "Route alerts" skill / Event Sinks API).
2. `createDetectionRule` (`POST /v1/detection_rule`) with required fields: `tenant_id`, `name`, `description`, `time_range_s`, `run_frequency_s` (<= `time_range_s`, minute granularity), `enabled_state_override` (`Active`|`Staging`|`Paused`), `severity` (Unknown/Information/Low/Medium/High/Critical/Fatal/Other), `query_text`, `event_sink_ids`. Optional: `alert_per_row`, `tags`, `alert_template`. An Active rule is immediately scheduled for backfill and execution.
3. `listDetectionRules` (`GET /v1/detection_rule`) to page through rules (cursor pagination: `pagination[page_size]`, `pagination[page_token]`); `getDetectionRule` (`GET /v1/detection_rule/{id}`) for full detail.
4. `updateDetectionRule` (`PUT /v1/detection_rule/{id}`) to change fields. For synced (Git-managed) rules only `enabled_state_override` is mutable.
5. `deleteDetectionRule` (`DELETE /v1/detection_rule/{id}`) to remove.

## Notes
- Rules-as-code can also be managed with the `scanner-cli` (validate/run-tests/sync) — see cli/scanner-cli.yml.
- Errors return `{ "error": "..." }`.
