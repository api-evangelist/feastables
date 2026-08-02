# Feastables

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
