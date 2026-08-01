---
name: Route Scanner alerts to Slack, PagerDuty, or a webhook
description: Create and manage event sinks so detection-rule alerts are delivered to the right destination.
api: openapi/scanner-openapi.yml
operations: [createEventSink, listEventSinks, getEventSink, updateEventSink, deleteEventSink]
---

# Route Scanner alerts to Slack, PagerDuty, or a webhook

Event sinks are the outbound destinations detection rules deliver alerts to.

## Auth
`Authorization: Bearer <Scanner API Key>`; include `tenant_id`.

## Steps
1. `createEventSink` (`POST /v1/event_sink`) with `tenant_id`, `name`, `description`, and `event_sink_args` configuring a Slack, Webhook, or PagerDuty destination. The response includes the sink `id` and `event_sink_type`.
2. `listEventSinks` (`GET /v1/event_sink`) / `getEventSink` (`GET /v1/event_sink/{id}`) to find existing sink ids.
3. Reference the sink `id`(s) in a detection rule's `event_sink_ids` (see the "Create and manage a detection rule" skill).
4. `updateEventSink` (`PUT /v1/event_sink/{id}`) / `deleteEventSink` (`DELETE /v1/event_sink/{id}`) to maintain.

## Notes
- The Webhook sink POSTs alert payloads to a URL you control; see asyncapi/scanner-webhooks.yml.
- Alert content/actions are shaped by the detection rule's `alert_template` ({{field_name}} interpolation).
