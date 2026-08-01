---
name: Summarize what is in an Anomalo-monitored table
description: >-
  Retrieve Anomalo's statistical data profile for a monitored table and explain what the table
  contains — column types, null rates, uniqueness, distributions and sampled values. Read-only.
api: openapi/anomalo-public-api-openapi.yml
operations:
  - get_active_organization_id
  - configured_tables
  - get_table_information
  - get_table_profile
  - get_table_documentation
generated: '2026-07-31'
method: generated
source: openapi/anomalo-public-api-openapi.yml + data-model/anomalo-data-model.yml + mcp/anomalo-mcp.yml
---

# Summarize what is in an Anomalo-monitored table

Read-only. Mirrors the `get_table_data_profile` tool and the `describe-table` slash command in
Anomalo's own Gemini CLI extension.

## Before you start

- Auth: `X-Anomalo-Token: <token>` against `https://{instance}/api/public/v1/`.
- Confirm the active organization with `GET /organization` (`get_active_organization_id`).
- **Data profiles exist only for tables Anomalo is configured to monitor.** An unmonitored table has no
  profile, and that is a normal answer — not an error to work around.

## Steps

1. **Resolve the table.** `GET /get_table_profile` (`get_table_profile`) accepts either `table_id` or a
   fully qualified `table_name` of the form `warehouse_name.schema.my_table`, so you can often go
   straight there.

   If you only have a partial name, list with `GET /configured_tables` (`configured_tables`) or resolve
   with `GET /get_table_information` (`get_table_information`) first.

2. **Read the profile.** The response carries:
   - `table_id`, `table_name`, `description` — the operator-supplied description of the table's use and
     contents, when set.
   - `column_value_samples` — sampled values per column, each with the value, its count, and its share
     of the column's sample.
   - `data_profile_details` — per-column statistics: data type, null percentage, uniqueness, min, max,
     mean, median, standard deviation, skew and kurtosis.
   - `column_value_image_url` and `data_profile_image_url` — rendered visualizations.

3. **Optionally pull the human documentation.** `GET /tables/{table_id}/documentation`
   (`get_table_documentation`) returns the curated documentation for the table, if any.

4. **Write the summary.** Lead with what the table appears to be *about*, grounded in the column names
   and sampled values. Then call out the columns that matter for data quality: high null rates, low
   uniqueness where you would expect a key, and heavily skewed distributions.

## Rules

- **Sampled, not exhaustive.** Every statistic comes from Anomalo's sample of the table. Say so.
  Do not present a sampled null rate or distinct count as an exact figure.
- **Do not infer semantics from column names alone.** Check the sampled values before asserting that a
  column holds emails, currency, identifiers or PII.
- **Treat sampled values as customer data.** Anomalo lets operators mark columns sensitive
  (`get_sensitive_columns`). Do not copy raw sampled values into logs, tickets or messages beyond what
  the operator asked for, and quote sparingly when illustrating a point.
- Present the image URLs as links; they are rendered assets, not data.
- If the profile is absent, report that the table is not configured for monitoring rather than
  retrying.
- Retry 5xx up to 5 times with exponential jitter backoff. Never retry 4xx. A 403 usually means the
  active organization or access-group policy does not cover the table.
