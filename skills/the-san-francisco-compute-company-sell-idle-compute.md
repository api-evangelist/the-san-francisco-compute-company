---
name: Resell idle compute capacity
description: >-
  Place a sell order on SF Compute to resell reserved compute you are not using,
  set a price floor, and monitor fills. Use for "sell compute", "resell GPUs",
  "recoup compute cost".
api: openapi/the-san-francisco-compute-company-openapi.json
base_url: https://api.sfcompute.com/preview/v2
operations:
- list_pools
- get_orderbook_quote
- get_order_preview
- create_order
- list_orders
- fetch_order
- cancel_order
- list_orderbook_fills
---

# Resell idle compute capacity

All requests go to `https://api.sfcompute.com/preview/v2` with a Bearer token.
Reselling lets you sell unused compute back into the market and recoup cost.

## Steps

1. **Find sellable balance** — `GET /pools` (`list_pools`) to see reserved
   balance you own and could resell.
2. **Check the market** — `GET /orderbook/quote` (`get_orderbook_quote`) to see
   what buyers are paying for that SKU/window.
3. **Preview** — `POST /order_preview` (`get_order_preview`) to sanity-check.
4. **Place a sell order with a floor** — `POST /orders` (`create_order`) with
   side `sell`, a count, and a minimum rate (price floor). Send an
   `Idempotency-Key` header.
5. **Monitor** — `GET /orders` (`list_orders`) / `GET /orders/{id}`
   (`fetch_order`), and `GET /orderbook/fills` (`list_orderbook_fills`) to watch
   fills.
6. **Cancel if needed** — `DELETE /orders/{id}` (`cancel_order`) to pull an
   unfilled order.

## Rules

- Set a `--min-rate` / minimum price floor so you never sell below your cost.
- You are never locked in — sell back only what you don't need.
- Idempotency-Key on `create_order`; handle the `{ error: { type, message } }`
  envelope for failures.
