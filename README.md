# Anomalo

Anomalo is an AI-powered data quality and data observability platform. It connects to cloud data
warehouses and lakehouses — Snowflake, Databricks, BigQuery, Redshift, S3 — profiles the tables it
monitors, and applies unsupervised machine learning to detect anomalies without hand-authored rules,
alongside declaratively configured validation checks. It covers structured, semi-structured and
unstructured data, performs root-cause analysis on failures, tracks table lineage, and routes alerts to
Slack, Microsoft Teams, Jira and ServiceNow.

- Website: https://www.anomalo.com/
- Product overview: https://www.anomalo.com/product-overview/
- GitHub: https://github.com/anomalo-hq

## API surface

Anomalo exposes a **Public REST API** at `https://{instance}/api/public/v1/`, authenticated with an API
secret token sent as the `X-Anomalo-Token` header. Anomalo is deployed **per tenant**, so there is no
single shared API host.

Anomalo's product and API documentation is **gated to customers**, and Anomalo publishes **no OpenAPI
description**. The two specs in `openapi/` were derived by the API Evangelist enrichment pipeline from
Anomalo's own Apache-2.0 licensed client code — the `anomalo` PyPI package (92 operations) and the
`anomalo-gemini-extension` MCP adapter (8 experimental unstructured operations). They are unofficial
and not endorsed by Anomalo. Every path, method and parameter is read mechanically from that source;
nothing is invented.

Anomalo also publishes an **official MCP server** (Apache-2.0), shipped as a Google Gemini CLI
extension. It is a local stdio server exposing 9 stable tools, 2 experimental tools and 4 MCP prompts —
a deliberate 17% read-mostly projection of the REST surface. See `mcp/anomalo-tool-crosswalk.yml` for
the tool-to-operation binding.

## Artifacts

| Directory | Contents |
|---|---|
| `openapi/` | Derived OpenAPI 3.1 for the Public API (92 ops) and the experimental Unstructured API (8 ops) |
| `overlays/` | API Evangelist analysis layered on the Public API spec |
| `mcp/` | Official MCP server manifest and the MCP↔REST tool crosswalk |
| `skills/` | Four packaged Agent Skills, grounded in verified operationIds |
| `packages/` | First-party Python client and Airflow provider |
| `cli/` | The `anomalo` CLI command surface, including configuration-as-code |
| `authentication/`, `scopes/` | API token auth; OIDC covers web sign-on only, not the API |
| `conventions/` | Tenancy, pagination, retries, versioning — read before integrating |
| `errors/` | Transport error contract and check-outcome fields |
| `data-model/` | 17-entity relationship graph |
| `lifecycle/`, `changelog/` | Versioning and the client release stream |
| `conformance/`, `security/`, `well-known/` | Standards conformance, domain security probes, `/.well-known/` survey |

## Notable findings

- **No idempotency.** No idempotency key, deduplication or conditional requests anywhere in the API.
- **Sticky tenancy.** The active organization is server-side state on the API key, not a request
  parameter — concurrent clients sharing a key interfere with each other.
- **No status page, no deprecation policy, no SLA, no public changelog, no security.txt** and no
  vulnerability disclosure program.
- **SOC 2 Type 2 and ISO 27001** per badges on https://www.anomalo.com/legal/.
- Domain security: TLS 1.3 and HSTS on both hosts, DMARC at `p=quarantine`, but **no SPF, no DNSSEC and
  no CAA**.
- `docs.anomalo.com` redirects to the WordPress marketing site, which returns HTTP 200 with rendered
  HTML for `/llms.txt` and every `/.well-known/*` path — a false-positive trap for discovery tooling.
