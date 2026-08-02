---
name: Buy compute and launch a GPU instance
description: >-
  Quote the market, place a buy order on SF Compute, wait for it to fill into a
  pool, then launch a GPU instance and get SSH access. Use for "reserve GPUs",
  "buy H100s", "launch an instance on SF Compute".
api: openapi/the-san-francisco-compute-company-openapi.json
base_url: https://api.sfcompute.com/preview/v2
operations:
- list_instance_sku_availability
- get_orderbook_quote
- get_order_preview
- create_order
- fetch_order
- create_instance
- fetch_instance
- get_instance_ssh
---

# Buy compute and launch a GPU instance

All requests go to `https://api.sfcompute.com/preview/v2` with
`Authorization: Bearer sk_live_...` (create a token with `sf tokens create`).
This API is in public preview.

## Steps

1. **See what's for sale** — `GET /instance_skus/availability` (`list_instance_sku_availability`)
   to find an `instance_sku` with capacity in your region/timeframe.
2. **Quote the market** — `GET /orderbook/quote` (`get_orderbook_quote`) for the
   current price on that SKU and window.
3. **Preview the order** — `POST /order_preview` (`get_order_preview`) to confirm
   cost before committing.
4. **Place the buy order** — `POST /orders` (`create_order`) with side `buy`,
   the `instance_sku`, count, and duration. Send an **`Idempotency-Key`** header
   so retries don't double-order. Orders fill asynchronously.
5. **Poll until filled** — `GET /orders/{id}` (`fetch_order`) until status is
   filled. A filled order increases your pool's reserved balance.
6. **Launch an instance** — `POST /instances` (`create_instance`) against the
   pool (optionally from an image/instance template).
7. **Wait for it to start** — `GET /instances/{id}` (`fetch_instance`) until running.
8. **Connect** — `GET /instances/{id}/ssh` (`get_instance_ssh`) for SSH host/key info.

## Rules

- Use `Idempotency-Key` on every `create_order` / `create_instance` POST.
- Handle errors via the `{ error: { type, message, details[] } }` envelope
  (see errors/the-san-francisco-compute-company-problem-types.yml): `402
  payment_required` means add credits; `403 forbidden` means the token needs a
  grant on the workspace.
- Creating/destroying instances does not affect billing — only orders do.
