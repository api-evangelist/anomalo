# Anomalo

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
