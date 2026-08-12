---
name: Find a heavy-duty part on FinditParts
description: Search the FinditParts catalog by part number, cross-reference or free text, then resolve the exact purchasable variant with its price, stock and ship cutoff.
api: openapi/finditparts-reseller-api-openapi.yml
operations:
  - lookupProductByPartNumber
  - productSearch
  - getProduct
  - getProductsMulti
  - variantLookup
method: generated
generated: '2026-08-12'
source: openapi/finditparts-reseller-api-openapi.yml
---

# Find a heavy-duty part on FinditParts

Catalog lookup on the FinditParts Reseller API. Read-only — nothing here places an order.

## Before you start

- Base URL is `https://finditparts.com/api/v1`. There is no sandbox host.
- Every request needs a **freshly signed HS256 JWT** in `Authorization: Bearer <token>`,
  signed with the Reseller Client Secret. Required claims: `iss` (Reseller Client ID)
  and `exp` (keep it short). Do not reuse a token across requests.
- **Pricing depends on the token.** Add `sub` (the Customer Reference) and
  `data.intent: "PRODUCT_SEARCH"` to get that customer's `account_price`. Omit `sub`
  and you get generic list price. Always tell the user which basis you returned.
- Set `Accept: application/json`.

## Steps

1. **If you have an exact part number, start with `lookupProductByPartNumber`.**
   `GET /products/lookup?part_number=<string>`. It returns two separate arrays —
   `part_number_matches` (the manufacturer's own number) and `cross_reference_matches`
   (equivalents from other manufacturers). Keep them distinct when you report back;
   a cross-reference is a substitution, not the same part.

2. **Otherwise search with `productSearch`.** `GET /products`. One of `part_number` or
   `query` is required. Narrow with `m` (manufacturer), `pc`/`psc`/`ppt` (category),
   `attrs`, `tags`, and `pf`/`pt` for a price range. Paginate with `page` and `per`.
   Read `pagination.total_count` to know how much you did not see.
   Results carry `match_score` and `match_relative_score` — rank by those rather than
   by array order, and never present a low-scoring match as a confirmed fit.

3. **Pull full detail with `getProduct`** (`GET /products/{product_id}`), or
   `getProductsMulti` (`GET /products/{product_ids}/multi`) for a comma-separated batch.
   Add `include_pies_data=true` when the user needs PIES attributes, interchange data
   or extended product information.

4. **Resolve the purchasable variant with `variantLookup`**
   (`GET /variants/{variant_ids}`). This is where the real commercial facts live:
   `price` and `account_price`, `quantity` in stock, `minimum` and `pack_quantity`,
   `core_price`, `cutoff` / `past_cutoff`, and the `estimated_*` ship and delivery
   dates. A product is not orderable information — a **variant** is.
   `preferred_variant_id` on the product is the default choice.

5. **Check `sellable` before you recommend anything.** A product can be in the catalog
   and not be for sale. Do not infer fitment or compatibility that the response does
   not state.

## Rules

- **`errno` rides on 200 responses.** Do not treat HTTP 200 as success. Read `errno`
  and treat any non-zero value as a failure. `errno: 1` here means no part number was
  provided; `errno: 404` means the id does not exist; `errno: 6` means the JWT is bad.
  Full registry: `errors/finditparts-error-codes.yml`.
- Errors come back as `{errno, message}` on HTTP 500 — not RFC 9457 problem details,
  and not a 4xx. A 500 from this API is usually your input, not an outage.
- `pack_quantity` and `minimum` are real constraints. Quoting a single unit price for a
  part that only ships in a pack of ten misleads the user.
- Never quote price or availability as durable. Re-read the variant before ordering.
- No rate limits are published. Do not fan out concurrent searches; go one at a time
  and back off on any 500.
