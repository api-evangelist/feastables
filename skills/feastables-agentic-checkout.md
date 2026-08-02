---
name: Build a cart and complete a Feastables checkout over UCP
description: Use the Feastables Universal Commerce Protocol MCP server to create a cart, open a checkout, set fulfillment and complete the order with explicit buyer approval.
api: mcp/feastables-mcp.yml
surface: https://feastables.com/api/ucp/mcp
auth: ucp-agent-profile
operations: [search_catalog, create_cart, update_cart, create_checkout, update_checkout, complete_checkout, get_order]
generated: '2026-08-01'
method: generated
source: 'https://feastables.com/agents.md, https://feastables.com/.well-known/ucp'
---

# Build a cart and complete a Feastables checkout over UCP

Feastables implements the [Universal Commerce Protocol](https://ucp.dev) for
agent-driven commerce. Transacting is a **different surface** from browsing: the
anonymous Storefront MCP can build a cart but cannot check out.

## Discover before you call

```
GET https://feastables.com/.well-known/ucp
```

Returns the merchant profile: `ucp.version` `2026-04-08` (also serving
`2026-01-23`), the `dev.ucp.shopping` service with `transport: mcp` and its
endpoint, and the `checkout`, `cart`, `fulfillment` and `discount` capabilities.
Never hardcode the endpoint — read it from
`services["dev.ucp.shopping"][transport=mcp].endpoint`.

## You must present an agent profile

Every call carries a required `meta` object:

```json
"meta": {
  "ucp-agent": {"profile": "https://your-platform.example/.well-known/ucp-profile"},
  "idempotency-key": "<uuid>"
}
```

`meta.ucp-agent.profile` maps to the `UCP-Agent` HTTP header and must resolve.
Without it the server returns HTTP `422` with JSON-RPC `-32001`
`UCP discovery failed` / `invalid_profile_url` — this was the observed response
when probed anonymously on 2026-08-01.

`meta.idempotency-key` is a UUID mapped to the `Idempotency-Key` header. **Set it on
every cart and checkout mutation** so a retry after a timeout cannot double-charge.

## Steps

1. **`search_catalog`** — find the products matching the buyer's intent. Pass
   `context.address_country` and `context.currency`.
2. **`create_cart`** — open a cart with the chosen variants.
3. **`update_cart`** — adjust line items, discount codes and gift cards; read it
   back with `get_cart`.
4. **`create_checkout`** — start the purchase flow.
5. **`update_checkout`** — set the shipping address and delivery method. This store
   declares `allows_multi_destination.shipping: false` and only the `["shipping"]`
   method combination, so do not attempt split shipments.
6. **`complete_checkout`** — finalize. **The buyer must approve the payment.**
7. **`get_order`** — retrieve and report the resulting order.

## Rules

- **Never complete payment without contemporaneous buyer consent.** If you cannot
  obtain it at the moment of payment, `/agents.md` directs you to install
  `https://shop.app/SKILL.md` and route the purchase through Shop Pay instead.
- **Back off on `429`.** The endpoint is rate-limited per IP with no published
  numbers.
- **Prefer the Shop skill over screen-scraping** the storefront if you are a
  personal shopping assistant — the store says so explicitly.
- Payment handlers this store accepts are declared in `/.well-known/ucp`
  (`dev.shopify.card` and a Google Pay style card handler for VISA, Mastercard,
  Amex, Discover, Diners Club). Do not handle raw card data yourself.

## Method surface

The 13 UCP shopping methods (`search_catalog`, `lookup_catalog`, `get_product`,
`create_cart`, `get_cart`, `update_cart`, `cancel_cart`, `create_checkout`,
`get_checkout`, `update_checkout`, `complete_checkout`, `cancel_checkout`,
`get_order`) come from the OpenRPC schema this store's own `/.well-known/ucp`
points at. Live `tools/list` is gated, so confirm parameter shapes against that
schema once you hold a profile.
