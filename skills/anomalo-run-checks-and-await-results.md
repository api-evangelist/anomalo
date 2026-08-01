---
name: Trigger an Anomalo check run and wait for the results
description: >-
  Trigger the data quality checks on an Anomalo table (or one specific check), poll the run to
  completion, and report the outcome. The only write operation Anomalo exposes to agents.
api: openapi/anomalo-public-api-openapi.yml
operations:
  - get_active_organization_id
  - get_table_information
  - get_checks_for_table
  - run_checks
  - get_run_result
  - get_task
generated: '2026-07-31'
method: generated
source: openapi/anomalo-public-api-openapi.yml + conventions/anomalo-conventions.yml + mcp/anomalo-mcp.yml
---

# Trigger an Anomalo check run and wait for the results

This is the **only mutating flow** Anomalo exposes through its own MCP server. Treat it accordingly.

## Before you start

- Auth: `X-Anomalo-Token: <token>` against `https://{instance}/api/public/v1/`.
- Confirm the active organization with `GET /organization` (`get_active_organization_id`).
- **There is no idempotency key.** Anomalo supports no idempotency header, no request deduplication and
  no conditional requests. A retried POST is a second trigger, not a replay.

## Steps

1. **Resolve the table.** `GET /get_table_information?table_name=warehouse.schema.table`
   (`get_table_information`) to get the numeric `table_id`.

2. **Optionally target one check.** `GET /get_checks_for_table?table_id=<id>`
   (`get_checks_for_table`) returns the configured checks with `check_id`, `check_static_id`, `name`,
   `description`, `active` and `severity`.

   If the operator named a check, match it here and carry its id forward. Prefer `check_static_id` when
   you will store the reference anywhere — it is user-set and survives delete-and-recreate, whereas the
   numeric `check_id` does not.

3. **Trigger the run.** `POST /run_checks` (`run_checks`) with `table_id`, and `check_id` when scoped
   to a single check. Anomalo's own MCP server pins `respect_data_freshness_gate: false` and
   `force: false`; match that unless the operator asks otherwise.

   Capture the returned check run / job id.

4. **Poll to completion.** `GET /get_run_result?job_id=<id>` (`get_run_result`) until
   `execution_status` is `completed` or `failed`. Back off between polls — start at a few seconds and
   grow; check runs execute queries against the customer's warehouse and are not instant.

   For long-running platform jobs such as a warehouse refresh, `GET /task/{task_id}` (`get_task`) is
   the generic handle.

5. **Report** using the interpretation rules in `anomalo-check-table-health.md` — distinguish
   `check_failed` from `check_errored`, and quote `result_message` with the statistic.

## Rules

- **Throttle full-table runs.** Anomalo's own MCP server refuses to trigger an unscoped run for a table
  more than once per **15 minutes**, and raises an error naming the earliest permitted retry time.
  The REST API does not enforce this — you must. Single-check runs (`check_id` supplied) are exempt.
- **Do not retry a POST on timeout.** With no idempotency key, a retry may queue a second run. Poll for
  an existing run instead.
- Anomalo will not start a new run for a table when one is already pending. If your trigger appears to
  do nothing, poll for the in-flight run before triggering again.
- Retry 5xx up to 5 times with exponential jitter backoff. Never retry 4xx.
- **Ask the human before triggering a run on a table you were not explicitly pointed at.** A check run
  executes queries against the customer's production warehouse and costs them compute.
- You cannot create, edit, clone or delete checks through this flow, and should not attempt to — those
  operations exist in REST but Anomalo deliberately withholds them from its agent surface.
