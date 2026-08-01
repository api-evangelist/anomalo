---
name: Triage a failed Anomalo check using table lineage
description: >-
  Investigate a failing data quality check by walking the upstream lineage of the affected table to
  find where the problem originated, and check whether upstream tables are failing too. Read-only.
api: openapi/anomalo-public-api-openapi.yml
operations:
  - get_active_organization_id
  - get_table_information
  - get_check_intervals
  - get_run_result
  - get_run_result_triage_history
  - list_table_upstream_lineage
  - list_table_downstream_lineage
  - get_checks_for_table
generated: '2026-07-31'
method: generated
source: openapi/anomalo-public-api-openapi.yml + data-model/anomalo-data-model.yml + mcp/anomalo-tool-crosswalk.yml
---

# Triage a failed Anomalo check using table lineage

Read-only. This flow has **no equivalent in Anomalo's own MCP server** — the four lineage operations
exist in the REST API but are not exposed as tools, so an agent working only through the published MCP
surface cannot do this. Working directly against the API, you can.

## Before you start

- Auth: `X-Anomalo-Token: <token>` against `https://{instance}/api/public/v1/`.
- Confirm the active organization with `GET /organization` (`get_active_organization_id`).
- Lineage is only as good as what has been recorded. Edges come from Anomalo's crawl and from explicit
  `table_lineage_edge` writes. **Absent lineage is not proof of independence.**

## Steps

1. **Establish the failure.** Resolve the table with `GET /get_table_information`
   (`get_table_information`), find its last completed run with `GET /get_check_intervals`
   (`get_check_intervals`), and read results with `GET /get_run_result` (`get_run_result`).

   Note which checks have `check_failed: true`, and capture `statistic_name`, `statistic_value` and
   `result_message` for each. Separate out any `check_errored: true` results — those are broken checks,
   a different investigation.

2. **Check whether this is new.** `GET /get_run_result_triage_history?run_checks_job_id=<job_id>`
   (`get_run_result_triage_history`) returns how this result has been triaged before. A failure that
   has been repeatedly triaged as expected is a different conversation from a first-time break.

3. **Walk upstream.** `GET /tables/{table_id}/lineage/upstream` (`list_table_upstream_lineage`),
   with `max_hops` to control depth and `limit` / `offset` to page.

   Start with `max_hops=1`. Widen only if the immediate parents are clean.

4. **Test each upstream table.** For each upstream table, repeat step 1: is it also failing, and did it
   fail *before* the table you started from? The earliest failing table in the chain is your candidate
   root cause.

   `GET /get_checks_for_table?table_id=<id>` (`get_checks_for_table`) tells you whether an upstream
   table is meaningfully monitored at all — a table with no active checks is a blind spot, not a clean
   bill of health. Say which it is.

5. **Assess blast radius.** `GET /tables/{table_id}/lineage/downstream`
   (`list_table_downstream_lineage`) from the root-cause table to list what else is affected.

6. **Report.** State the root-cause candidate, the evidence chain (table, check, statistic, timestamp
   at each hop), the downstream blast radius, and your confidence.

## Rules

- **Correlation is not causation.** Upstream failing at a similar time is evidence, not proof. Say
  "consistent with" rather than "caused by" unless the statistics line up directly.
- **Respect timestamps.** Compare `completed_at` across hops. An upstream failure detected *after* the
  downstream one may be a separate incident.
- **Distinguish three states** for every upstream table: failing, passing, and not-monitored. Collapsing
  the third into the second is the most common error in this flow.
- Bound your walk. Do not traverse the full graph — start at `max_hops=1` and widen deliberately.
  Lineage lists are paginated with `limit` / `offset`; respect that rather than pulling everything.
- Do not fetch the bad-row CSV links; they need an authenticated browser session. Hand them to the
  human, along with `sql_for_bad_rows` for them to run in their own warehouse.
- This flow is read-only. **Do not** trigger check runs to "refresh" upstream tables while triaging —
  that costs the customer warehouse compute. If you need fresh results, ask first, then follow
  `anomalo-run-checks-and-await-results.md` and its 15-minute per-table throttle.
- Retry 5xx up to 5 times with exponential jitter backoff. Never retry 4xx.
