---
name: Browse and search the Feastables catalog
description: Search the Feastables storefront, inspect a product and its variants, and answer store-policy questions using the anonymous Storefront MCP server.
api: mcp/feastables-mcp.yml
surface: https://feastables.com/api/mcp
auth: none
operations: [search_catalog, get_product_details, search_shop_policies_and_faqs]
generated: '2026-08-01'
method: generated
source: 'https://feastables.com/api/mcp tools/list (probed 2026-08-01)'
---

# Browse and search the Feastables catalog

Feastables serves a Model Context Protocol server from its own origin at
`https://feastables.com/api/mcp`. It answers `initialize` and `tools/list` with **no
credential**, so read-only catalog work needs no onboarding at all.

## Connect

```json
POST https://feastables.com/api/mcp
Content-Type: application/json
Accept: application/json, text/event-stream

{"jsonrpc":"2.0","id":1,"method":"initialize",
 "params":{"protocolVersion":"2025-06-18","capabilities":{},
           "clientInfo":{"name":"your-agent","version":"1.0"}}}
```

The server identifies as `storefront-renderer` 0.1.0 and negotiates MCP
`2025-06-18`. It advertises `prompts` and `resources` capabilities, but both
`prompts/list` and `resources/list` return empty arrays — do not build on them.

## Steps

1. **`search_catalog`** — pass a natural-language `query`, structured `filters`, or
   both. At least one is required. Results are limited on the first page; take
   `pagination.cursor` from the response and pass it back through `catalog` to page
   further. Set `context.address_country` and `context.currency` so pricing and
   availability are correct for the buyer.
2. **`get_product_details`** — required `product_id`. Pass `options` to pin a
   specific variant; without it the first available variant is returned. `country`
   and `language` localize the response.
3. **`search_shop_policies_and_faqs`** — required `query`. Use it for return
   policy, shipping policy, contact details and hours instead of scraping
   `/pages/faq` or `/policies/*`.

## Rules

- **Rate limits are per IP** and unquantified. Back off on `429`; do not retry
  tightly.
- The Storefront MCP tools declare **no idempotency field** — idempotency
  (`meta.idempotency-key`) exists only on the gated UCP surface. Treat repeated
  `update_cart` calls as non-idempotent.
- Unknown paths return an **HTML** 404, not JSON. Check `Content-Type`, not just
  status.
- Product IDs and variant IDs are 64-bit integers; the public JSON endpoints key on
  the URL `handle` instead. See `data-model/feastables-data-model.yml`.

## No-MCP fallback

Feastables documents read-only JSON endpoints in `/agents.md`:
`GET /products.json`, `GET /products/{handle}.json`,
`GET /collections/{handle}/products.json`, `GET /search?q={query}&type=product`.
These are anonymous and return the same catalog entities, without natural-language
search or cursor pagination.
