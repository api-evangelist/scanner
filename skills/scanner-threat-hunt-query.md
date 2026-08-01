---
name: Run a Scanner threat-hunt query
description: Execute an ad hoc full-text query over Scanner-indexed security logs and retrieve the results.
api: openapi/scanner-openapi.yml
operations: [listIndexes, startQuery, getQueryProgress, cancelQuery, blockingQuery]
---

# Run a Scanner threat-hunt query

Use this to search indexed security logs for a time window (threat hunts, IOC checks, investigations).

## Auth
Send `Authorization: Bearer <Scanner API Key>` (Settings > API Keys). Most calls need a `tenant_id`.

## Steps
1. (Optional) `listIndexes` (`GET /v1/index?tenant_id=...`) to discover searchable indexes.
2. Submit the query with `startQuery` (`POST /v1/start_query`): body `query`, `start_time`, `end_time` (RFC 3339; end is exclusive), optional `max_rows` (default 1000, max 100000) and `max_bytes` (default/max 128MB). You get back `{ qr_id }`.
3. Poll `getQueryProgress` (`GET /v1/query_progress/{qr_id}`) about once per second until `is_completed` is true. Read `results.column_ordering` and `results.rows`.
4. To abort, call `cancelQuery` (`POST /v1/cancel_query/{qr_id}`) — idempotent, returns 204.

## Notes
- For short queries you may use `blockingQuery` (`POST /v1/blocking_query`) instead of poll-based flow; it holds the connection open up to 300s and returns 504 on timeout — prefer the async flow for large scans.
- Results are capped at `max_bytes`; rows may be dropped beyond that regardless of `max_rows`.
- Errors return `{ "error": "..." }` (see errors/scanner-problem-types.yml).
