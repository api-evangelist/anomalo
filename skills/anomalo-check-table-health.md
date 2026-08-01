---
name: Check the data quality health of an Anomalo table
description: >-
  Find a monitored table in Anomalo by name or label, read the results of its most recent data quality
  check run, and explain which checks failed and why. Read-only.
api: openapi/anomalo-public-api-openapi.yml
operations:
  - get_active_organization_id
  - get_all_organizations
  - set_active_organization_id
  - list_labels_for_organization
  - configured_tables
  - get_table_information
  - tables
  - get_check_intervals
  - get_run_result
generated: '2026-07-31'
method: generated
source: openapi/anomalo-public-api-openapi.yml + conventions/anomalo-conventions.yml + mcp/anomalo-mcp.yml
---

# Check the data quality health of an Anomalo table

Read-only. This is the flow Anomalo's own MCP server exposes as `get_configured_tables` +
`get_table_check_status`.

## Before you start

- **Base URL** is per-tenant: `https://{instance}/api/public/v1/`. There is no shared host. Get the
  instance from the operator (`ANOMALO_INSTANCE_HOST`).
- **Auth**: send the API secret token as `X-Anomalo-Token: <token>`. (`Authorization: Bearer <token>`
  also works.) There are no scopes.
- **Confirm your organization first.** Anomalo scopes every call to a *sticky server-side* active
  organization tied to the API key — not to a request parameter. Never assume it.

## Steps

1. **Confirm scope.** `GET /organization` (`get_active_organization_id`) to read the active
   organization. If it is not the one you want, `GET /organizations` (`get_all_organizations`) to list
   them, then `PUT /organization` (`set_active_organization_id`) with `{"id": <org_id>}`.

   Warn the operator before switching: this mutates state on the API key, so any other client sharing
   that key has its scope changed too.

2. **Locate the table.**
   - By fully qualified name: `GET /get_table_information?table_name=warehouse.schema.table`
     (`get_table_information`). It also accepts `table_id` or `warehouse_id`.
   - By label: `GET /org_labels` (`list_labels_for_organization`) to resolve the label name or slug to
     an id, then `GET /tables?label_id=<id>` (`tables`).
   - To browse everything monitored: `GET /configured_tables` (`configured_tables`).

   `configured_tables` and `tables` are **not paginated**. Expect the whole collection.

3. **Find the most recent completed check run.** `GET /get_check_intervals?table_id=<id>`
   (`get_check_intervals`), optionally bounded by `start` and `end`. This operation pages by time
   window, not by count.

4. **Read the results.** `GET /get_run_result?job_id=<id>` (`get_run_result`), or pass `check_run_id`.
   You get a `check_runs` array.

5. **Interpret each result.** A 200 response does not mean the data is healthy — check outcomes are
   fields on the payload, not HTTP status:
   - `check_passed` / `check_failed` — the data quality verdict.
   - `check_errored` + `exception_msg` — the check itself broke. This is **not** a data quality
     failure; say so explicitly rather than reporting the table as unhealthy.
   - `check_skipped` — did not run.
   - `execution_status` — `pending`, `running`, `completed` or `failed`. Only trust results from a
     completed run.
   - `result_message`, `check_history_message`, `statistic_name`, `statistic_value`,
     `statistic_context` — the explanation and the number behind it.

6. **Report.** Summarize passed / failed / errored counts, then detail each failure with its
   `result_message` and statistic.

## Rules

- **Do not fetch the CSV links.** `url_for_csv_of_sample_of_bad_rows` and
  `url_for_csv_of_sample_of_good_rows` require an authenticated browser session and will not work
  programmatically. Surface them to the human as links instead.
- `sql_for_bad_rows` and `sql_for_good_rows` are queries the operator can run in their own warehouse.
  Present them; do not execute them against Anomalo.
- **Never conflate an errored check with a failed check** when reporting.
- On 5xx, retry up to 5 times with exponential backoff and jitter (1s initial, 10s cap). On 4xx, stop —
  4xx is never retried, and the body is opaque text, not a structured error.
- 403 usually means the active organization is wrong or the caller's access-group policy does not cover
  the table. Re-check step 1 before concluding the table does not exist.
