---
name: Quote shipping for a FinditParts order
description: Get carrier options, cost and delivery estimates for a proposed set of FinditParts line items and a destination address, on either the reseller or the partner surface.
api: openapi/finditparts-reseller-api-openapi.yml
operations:
  - shippingMethods
  - partnersShippingMethods
  - getCartShippingMethods
  - variantLookup
method: generated
generated: '2026-08-12'
source: openapi/finditparts-reseller-api-openapi.yml
---

# Quote shipping for a FinditParts order

Turn a set of parts plus a destination into carrier options with real cost and dates.

## Pick the right operation

| Situation | Operation |
|---|---|
| Quoting before a cart exists (reseller JWT or API key) | `shippingMethods` — `GET /shipping_methods` |
| Quoting on the Partner API-key surface | `partnersShippingMethods` — `POST /partners/shipping_methods` |
| A cart already exists | `getCartShippingMethods` — `GET /carts/{cart_id}/shipping_methods` |

## Two ways to authenticate `shippingMethods`

**API key.** `Authorization: Bearer api-XYZ123`, with line items and address as query
parameters in Rails bracket form:

```
GET /api/v1/shipping_methods.json?line_items[][variant_id]=123&line_items[][quantity]=1&address[zipcode]=93101
```

Multiple items by product id:

```
GET /api/v1/shipping_methods.json?line_items[][finditparts_product_id]=456&line_items[][quantity]=1&line_items[][finditparts_product_id]=123&line_items[][quantity]=1&address[zipcode]=93101
```

**JWT.** Sign with the Reseller Client Secret. The `sub` claim (Customer Reference) is
**required** on this call. You may put the payload in the JWT instead of the query
string:

```
JWT.sign({
  sub: "CUSTOMER_123",
  iss: "<your Reseller Client ID>",
  exp: <short expiry>,
  data: {
    intent: "SHIPPING_METHODS",
    line_items: [{ variant_id: '...', quantity: '...' }],
    address: {
      address1: '...', address2: '...', city: 'Los Angeles', state: 'CA',
      zipcode: '90000', country: 'USA', address_type: "commercial"
    }
  }
}, 'RESELLER_CLIENT_SECRET')
```

## Steps

1. **Resolve every line item to a variant first** with `variantLookup`. Quantity must
   respect the variant's `minimum` and `pack_quantity`, or the quote will not match
   what is orderable. Line items may be addressed by `variant_id` or by
   `finditparts_product_id`.
2. **Set `address_type` deliberately** — `commercial` or `residential`. It changes the
   price. If you do not know, ask; do not default silently.
3. **Call the quote.** You get back `shipping_methods[]` with `id`, `label`, `amount`,
   `currency`, `carrier_type`, `days_to_deliver`, `estimated_delivery_date` and
   `available`.
4. **Filter on `available`.** An unavailable method is listed but cannot be selected.
5. **Keep the `id`.** It is what you pass to `selectCartShippingMethod`, or as
   `default_shipping_method` in a `NEW_ORDER` reseller customer session.

## Rules

- Check `errno` on every response even at HTTP 200. `errno: 2` is an invalid address —
  fix the fields, do not retry unchanged. `errno: 3` means a requested variant is not
  sellable. Full registry: `errors/finditparts-error-codes.yml`.
- `manufacturer` appears on a shipping method because parts can ship from different
  origins; a multi-manufacturer basket may quote several shipments.
- Costs are estimates tied to the exact line items and address you sent. Re-quote if
  either changes.
- Cross-reference the variant's `cutoff` and `past_cutoff` before promising a ship
  date — a quote's `estimated_delivery_date` assumes the order clears today's cutoff.
