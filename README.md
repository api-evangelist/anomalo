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

Anomalo is an AI-powered data quality and data observability platform that automatically detects, alerts on, triages and helps resolve data issues before they reach analytics, reporting or AI models. It connects directly to cloud data warehouses and lakehouses — Snowflake, Databricks, BigQuery, Redshift, Amazon S3 and others — profiles the tables it monitors, and applies unsupervised machine learning to surface anomalies without requiring engineers to hand-author rules or maintain thresholds, alongside declaratively configured validation checks. The platform covers structured, semi-structured and unstructured data, performs root-cause analysis on failed checks, tracks table lineage, and routes alerts to Slack, Microsoft Teams, Jira, ServiceNow, PagerDuty and email. Anomalo exposes a REST Public API (`/api/public/v1/`) for warehouse connection, table configuration, check authoring and execution, check-run results, labels, lineage edges, users, API keys and access-group administration, plus a first-party Python client and CLI, an Apache Airflow provider, and an official MCP server shipped as a Google Gemini CLI extension. Anomalo is deployed per-tenant, with API documentation gated to customers; the company holds SOC 2 Type 2 and ISO 27001 attestations.

**Areas:** Data Quality · Data Observability · Data Monitoring · Anomaly Detection · Data Governance · Data Lineage · Data Profiling · Data Validation · Data Engineering · Machine Learning · Snowflake · Databricks

## Anomalo on APIs.io

Everything profiled for Anomalo is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/anomalo/](https://apis.io/providers/anomalo/)** — or go straight to one of the APIs or properties below.

### APIs (19)

- [Anomalo MCP Server](https://apis.io/apis/anomalo/anomalo-mcp-server/) — Official Anomalo MCP (Model Context Protocol) server, published by Anomalo as a Google Gemini CLI extension under the Apache-2.0 license. Runs locally over…
- [Anomalo Access Groups API](https://apis.io/apis/anomalo/anomalo-access-groups-api/) — The Access Groups API from Anomalo — 10 operation(s) for access groups.
- [Anomalo API Keys API](https://apis.io/apis/anomalo/anomalo-api-keys-api/) — The API Keys API from Anomalo — 2 operation(s) for api keys.
- [Anomalo Check Runs API](https://apis.io/apis/anomalo/anomalo-check-runs-api/) — The Check Runs API from Anomalo — 2 operation(s) for check runs.
- [Anomalo Checks API](https://apis.io/apis/anomalo/anomalo-checks-api/) — The Checks API from Anomalo — 9 operation(s) for checks.
- [Anomalo Document Collections API](https://apis.io/apis/anomalo/anomalo-document-collections-api/) — Sets of unstructured documents Anomalo monitors and analyzes.
- [Anomalo Documents API](https://apis.io/apis/anomalo/anomalo-documents-api/) — Individual documents within a collection, and the issues found in them.
- [Anomalo Exports API](https://apis.io/apis/anomalo/anomalo-exports-api/) — The Exports API from Anomalo — 1 operation(s) for exports.
- [Anomalo Issues API](https://apis.io/apis/anomalo/anomalo-issues-api/) — Data quality issues detected across an organization's unstructured content.
- [Anomalo Labels API](https://apis.io/apis/anomalo/anomalo-labels-api/) — The Labels API from Anomalo — 5 operation(s) for labels.
- [Anomalo Lineage API](https://apis.io/apis/anomalo/anomalo-lineage-api/) — The Lineage API from Anomalo — 3 operation(s) for lineage.
- [Anomalo Notification Channels API](https://apis.io/apis/anomalo/anomalo-notification-channels-api/) — The Notification Channels API from Anomalo — 3 operation(s) for notification channels.
- [Anomalo Organizations API](https://apis.io/apis/anomalo/anomalo-organizations-api/) — The Organizations API from Anomalo — 3 operation(s) for organizations.
- [Anomalo Sensitive Columns API](https://apis.io/apis/anomalo/anomalo-sensitive-columns-api/) — The Sensitive Columns API from Anomalo — 2 operation(s) for sensitive columns.
- [Anomalo System API](https://apis.io/apis/anomalo/anomalo-system-api/) — The System API from Anomalo — 1 operation(s) for system.
- [Anomalo Tables API](https://apis.io/apis/anomalo/anomalo-tables-api/) — The Tables API from Anomalo — 10 operation(s) for tables.
- [Anomalo Tasks API](https://apis.io/apis/anomalo/anomalo-tasks-api/) — The Tasks API from Anomalo — 1 operation(s) for tasks.
- [Anomalo Users API](https://apis.io/apis/anomalo/anomalo-users-api/) — The Users API from Anomalo — 6 operation(s) for users.
- [Anomalo Warehouses API](https://apis.io/apis/anomalo/anomalo-warehouses-api/) — The Warehouses API from Anomalo — 5 operation(s) for warehouses.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [Anomalo MCP Server](https://apis.io/servers/anomalo/anomalo-mcp-server/)

#### Collections (2)

Postman and Open Collection request collections.

- [Anomalo Public API](https://apis.io/collections/anomalo/open-anomalo-public-api/)
- [Anomalo Unstructured Data API](https://apis.io/collections/anomalo/open-anomalo-unstructured/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Anomalo Public Api Overlay](https://apis.io/overlays/anomalo/anomalo-public-api-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Anomalo Authentication](https://apis.io/security/anomalo/anomalo-authentication/)
- [Anomalo Domain Security](https://apis.io/security/anomalo/anomalo-domain-security/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Anomalo Scopes](https://apis.io/scopes/anomalo/anomalo-scopes/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.anomalo.com/)
- [Documentation](https://www.anomalo.com/product-overview/)
- [Sign up](https://www.anomalo.com/request-a-demo/)
- [Support](https://www.anomalo.com/request-support/)
- [Blog](https://www.anomalo.com/blog/)
- [Compliance](https://www.anomalo.com/legal/)
- [Terms of service](https://www.anomalo.com/legal/subscription/)
- [Privacy policy](https://www.anomalo.com/legal/privacy/)
- [GitHub](https://github.com/anomalo-hq)

## API Evangelist rating

**Kin Score 35.6** — thin · rubric 0.24.0 · scored 2026-10-17

| Facet | Score |
|---|---|
| Discoverability | 78.6 |
| Contract quality | 15.4 |
| Contract governance | 4.5 |
| Operational transparency | 18.4 |
| Developer ergonomics | 44.6 |
| Access clarity | 42.1 |

**Agent readiness 21.0** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | derived (partial credit) |
| OpenAPI Examples | — |
| Rate Limit Signal | — |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |

**Regulatory alignment 36.4** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/anomalo/](https://apis.io/providers/anomalo/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Anomalo: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/anomalo/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
