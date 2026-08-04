# Feastables

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
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Feastables is the consumer snack brand founded by Jimmy Donaldson (MrBeast) — chocolate bars,
peanut butter cups, sour gummies, milk and variety boxes, sold direct at feastables.com and across
roughly 30,000 retail locations in the US, Canada and Mexico.

Feastables ships **no developer API and no OpenAPI**. What it does ship — and what this repo
profiles — is a real agentic-commerce surface on its own origin:

| Surface | URL | Status |
|---|---|---|
| Agent instructions | https://feastables.com/agents.md | 200, canonical, listed in its own agentic-discovery sitemap |
| llms.txt | https://feastables.com/llms.txt | 200, mirrors agents.md |
| UCP merchant profile | https://feastables.com/.well-known/ucp | 200, UCP `2026-04-08` + `2026-01-23` |
| Storefront MCP | https://feastables.com/api/mcp | 200 anonymous — 5 tools with real inputSchema |
| UCP Shopping MCP | https://feastables.com/api/ucp/mcp | 422 — gated on a UCP-Agent profile URI |
| OAuth 2.0 / OIDC discovery | `/.well-known/openid-configuration`, `/oauth-authorization-server`, `/oauth-protected-resource` | 200, Shopify customer accounts |
| Read-only catalog JSON | `/products.json`, `/products/{handle}.json`, `/collections/{handle}/products.json`, `/search` | 200, anonymous |

Not present (probed 2026-08-01 on `feastables.com` and `feastables.myshopify.com`): OpenAPI,
AsyncAPI, A2A agent card, `security.txt`, `api-catalog`, `ai-plugin.json`, first-party SDKs,
CLI, changelog, status page.

- Website — https://feastables.com
- Secondary market listing — https://forgeglobal.com/feastables_stock/
