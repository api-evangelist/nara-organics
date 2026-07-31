---
name: Buy from Nara Organics (UCP agent commerce)
description: Discover, cart, and check out Nara Organics products as an AI shopping agent over the store's Universal Commerce Protocol (UCP) MCP endpoint, with buyer-approved payment.
api: mcp/nara-organics-mcp.yml
operations: [search_catalog, create_cart, create_checkout, update_checkout, complete_checkout]
source: https://nara.com/llms.txt
method: searched
---

# Buy from Nara Organics as an agent

Nara Organics is a Shopify store that implements the Universal Commerce Protocol (UCP)
for agent-driven commerce. Transact over the MCP endpoint; browsing needs no auth.

## Endpoints
- UCP discovery: `GET https://nara.com/.well-known/ucp`
- MCP endpoint: `POST https://nara-organics.myshopify.com/api/ucp/mcp` (`Content-Type: application/json`)
- Read-only product data: `GET https://nara.com/products/{handle}.json`

## Steps
1. **Discover** — `GET /.well-known/ucp` to confirm supported versions and capabilities.
   Use MCP `tools/list` to fetch the live tool schemas.
2. **Search** — call `search_catalog` with the buyer's intent. Pass `context.address_country`
   and `context.currency` for accurate pricing and availability.
3. **Cart** — call `create_cart` to add the chosen items.
4. **Checkout** — call `create_checkout` to start the purchase flow.
5. **Fulfill** — call `update_checkout` to set the shipping address and method.
6. **Complete** — call `complete_checkout`. **The buyer must explicitly approve payment.**

## Rules
- **Human approval is mandatory at payment.** Never complete a purchase without contemporaneous
  buyer consent. If you cannot get it, route through Shop Pay (`https://shop.app/SKILL.md`).
- **Respect rate limits.** The MCP endpoint is rate-limited per IP; back off on HTTP 429.
- **Payment handlers:** Google Pay, Shopify card, and Shop Pay — the agent never handles raw card data.
- **Note:** Confirm current product availability first — Nara has published a voluntary infant-formula recall.
