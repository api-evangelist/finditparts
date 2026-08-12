---
name: Build a cart and check out on FinditParts
description: Drive the direct API-key path — exchange user credentials for a session JWT, build a cart, set addresses and shipping, then complete the order and reconcile it.
api: openapi/finditparts-reseller-api-openapi.yml
operations:
  - createSession
  - refreshSession
  - createCart
  - addCartLineItem
  - changeCartLineItems
  - setCartShippingAddress
  - getCartShippingMethods
  - selectCartShippingMethod
  - setCartPoNumber
  - completeCartWithCreditCard
  - completeCartWithCorporateBilling
  - getCart
  - searchOrders
  - getOrder
method: generated
generated: '2026-08-12'
source: openapi/finditparts-reseller-api-openapi.yml
---

# Build a cart and check out on FinditParts

The direct path, for API-key clients that own the buying UI. This flow places real
orders for real money. Treat the checkout step as one-way.

## Get a user-specific session first

An API key alone cannot touch carts or orders. Exchange credentials for a user JWT:

`createSession` — `POST /sessions` with `{"email": "...", "password": "..."}` and the
API key in `Authorization: Bearer api-XYZ123`. Returns `{jwt, user}`. Use that `jwt` as
`Authorization: Bearer USER.JWT.TOKEN` on everything below.

User session JWTs default to **1 month**. Call `refreshSession`
(`POST /sessions/refresh`) with the current, unexpired token before it lapses.
`getCurrentSession` (`GET /sessions/current`) tells you whose session you hold.

## Steps

1. **`createCart`** — `POST /carts`. Returns `cart.id` and a `cart_token`. Cart
   operations accept either the user JWT or that cart token.
2. **`addCartLineItem`** — `POST /carts/{cart_id}/line_items` with
   `{finditparts_product_id, variant_id, quantity}`. Resolve the variant first
   (see the find-a-part skill) and respect `minimum` and `pack_quantity`.
3. **`changeCartLineItems`** — `PUT /carts/{cart_id}/line_items` with
   `{line_items_attributes: [{id, quantity}, ...]}` to adjust quantities. Note these
   are **line item ids**, not variant ids.
4. **`setCartShippingAddress`** — `PUT /carts/{cart_id}/shipping_address`. Pass either
   an `address_id` from the address book or an inline `address` object. Set
   `address_type` (`commercial` or `residential`) deliberately; it changes cost.
   `setCartBillingAddress` is the same shape on `/billing_address`.
5. **`getCartShippingMethods`** — `GET /carts/{cart_id}/shipping_methods`. Filter to
   `available: true`, then **`selectCartShippingMethod`** —
   `PUT /carts/{cart_id}/shipping_methods` with `{shipping_method_id}`.
6. **`setCartPoNumber`** (optional) — `PUT /carts/{cart_id}/po_number` with
   `{po_number}`. Fleet buyers usually need this on the order.
7. **Re-read the cart with `getCart` before checkout.** Confirm `total`, `item_total`,
   `line_items`, `adjustments` and the selected shipping method against what you showed
   the user. Prices move.
8. **Complete the order.**
   - `completeCartWithCreditCard` — `POST /carts/{cart_id}/complete_with_credit_card`
     with `{payment_method_nonce, device_data}`. The nonce comes from a client-side
     tokenization component using the `payment_processor_client_token` returned on the
     user object — the API never sees a card number, and neither should you.
   - `completeCartWithCorporateBilling` —
     `POST /carts/{cart_id}/complete_with_corporate_billing`, only when the cart
     reports `corporate_billing_available: true`.
9. **Reconcile.** Read the returned cart, then confirm with `getOrder` or `searchOrders`
   (`GET /orders/search?q=...&start_date=...&end_date=...`).

## Rules — read before step 8

- **There is no idempotency key on this API.** If a checkout call times out or errors
  ambiguously, **do not retry it.** Call `searchOrders` or `getOrder` and find out
  whether the order landed. A blind retry can charge the customer twice.
- **`errno` rides on 200.** A `200 OK` from `completeCartWithCreditCard` carrying
  `errno: 4` is a failed order. Check `errno` first, always.
- `errno: 4` = the line items or variant do not meet requirements. `errno: 3` = a
  variant is no longer sellable — re-resolve it. `errno: 2` = the address is invalid.
  `errno: 6` = the session JWT expired; refresh and start again from the cart read, not
  from checkout. Full registry: `errors/finditparts-error-codes.yml`.
- Errors arrive as HTTP 500 with `{errno, message}`. A 500 here is usually your input.
- No rate limits are published and no `Retry-After` is returned. Serialize your calls
  and back off manually.
- If you are acting on behalf of a customer rather than owning the buying UI, prefer the
  hosted `NEW_ORDER` session in the onboard-a-reseller-customer skill — it puts a human
  on the confirmation screen.
