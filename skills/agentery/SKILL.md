---
name: agentery
description: Use Agentery to find AI tools, agents and MCP servers for a task, compare their published prices, and check whether a price is fair.
---

# Agentery

Use the Agentery MCP server when the user is choosing an AI product, AI agent,
MCP server or API, comparing what such products cost, or judging a price or quote.

## Tool selection

- New selection request: start with `search_providers`, inspect promising
  candidates with `get_provider_profile`, then compare a shortlist of 2–6 with
  `compare_providers`, using the returned provider handles.
- Fair price for a type of product: `price_benchmark` (takes `query`) or
  `get_group_fair_price`.
- Market context: `market_report` or `find_market`. `research_capability`
  combines market, pricing context and a shortlist in one call.
- Substitutes for one known product: `suggest_alternatives`.
- Price trends: `get_price_index` / `get_price_index_history` return the Agent
  Economy Price Index — an index level, not a price for any product.
- A specific question about one product (for example "is there a free trial?"):
  `request_research`, then `get_research`.

## Reading results

1. A missing price is unknown, not zero. "Contact sales" is not a price.
2. A search result is a candidate to check, not proof that it meets every
   requirement. State task fit, delivery type and billing unit before
   recommending anything.
3. Keep source links and observation dates with any price you quote, and tell
   the user to confirm the price with the vendor before buying.

## Write tools

Only call `report_outcome` after the user has actually used a listed product.
Private benchmark tools (`create_`/`update_`/`delete_custom_benchmark`) return a
private token; give it to the user and do not repeat it elsewhere. The server
needs no credentials, so never ask the user for an API key.
