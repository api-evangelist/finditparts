---
name: Onboard and order for a reseller customer on FinditParts
description: Link one of your own customers to a FinditParts account with a hosted USER_SETUP session, then place an order for them with a hosted NEW_ORDER session and confirm the result server-side.
api: openapi/finditparts-reseller-api-openapi.yml
operations:
  - createResellerCustomer
  - createResellerCustomerSession
  - getResellerCustomerSession
  - cancelResellerCustomerSession
  - listResellerCustomers
  - shippingMethods
method: generated
generated: '2026-08-12'
source: openapi/finditparts-reseller-api-openapi.yml
---

# Onboard and order for a reseller customer on FinditParts

The reseller flow. A human always completes the hosted page — this skill drives the
API around that handoff and confirms the outcome. It never places an order silently.

## The key concept: Customer Reference

A **Customer Reference** is *your* unique handle for *your* customer — a user id or an
email. It is carried as the JWT `sub` claim and is the join key for everything below.
You must complete a `USER_SETUP` session for a Customer Reference **before** you can
run a `NEW_ORDER` session for it.

## Steps

### 1. Create the reseller customer

`createResellerCustomer` — `POST /reseller_customers` with `{"customer_reference": "..."}`,
signed with a Master Account JWT. Returns `{customer_reference, user_id}`.
`listResellerCustomers` (`GET /reseller_customers`, Master Account API key) enumerates
existing ones — check there first so you do not re-create.

### 2. Run a USER_SETUP session

`createResellerCustomerSession` — `POST /reseller_customer_sessions`. Sign a JWT with
`sub` = the Customer Reference and a `data` payload:

```
data: {
  intent: "USER_SETUP",
  user: { firstname, lastname, email, company, address: { address1, address2, city, zipcode, state, phone, country } },
  redirect_url: "https://yoursite.example/finditparts_setup_success"
}
```

`user` is optional and only prefills the form. The response carries
`reseller_customer_session.redirect_url` — a finditparts.com URL. **Render it to the
human**, in an iframe or as a full redirect. The customer creates or signs into a
FinditParts account and puts a payment method and shipping address on file.

You may re-run `USER_SETUP` on an already-linked Customer Reference to let them change
payment details or addresses.

### 3. Catch the completion

- **With `redirect_url`:** FinditParts appends
  `resolution=SUCCESS&customer_reference=XXX&reseller_customer_session_id=XXX`, or on
  failure `resolution=FAILED&reason=XXX&...`.
- **Without `redirect_url` (iframe):** the frame posts
  `{ pageLoaded: true }` on load, then `{ error: false, intent, resolution, reseller_customer_session_id, customer_reference }`
  on completion, or `{ error: true, message }`.

**Validate `event.origin` against the FinditParts origin.** The provider's docs do not
specify a target origin, and an unvalidated `postMessage` listener will accept anything.

### 4. Confirm server-side — always

Call `getResellerCustomerSession` — `GET /reseller_customer_sessions/{reseller_customer_session_id}`
with a **generic** JWT (no `sub`). The browser told you what it wanted to; this is the
only trustworthy confirmation. Read `resolution`.

### 5. Place an order with a NEW_ORDER session

Quote first with `shippingMethods` to get a `default_shipping_method` id, then
`createResellerCustomerSession` again with `sub` = the Customer Reference and:

```
data: {
  intent: "NEW_ORDER",
  line_items: [
    { finditparts_product_id: "...", quantity: 1 },
    { variant_id: "...", quantity: 2 },
    { part_number: "...", manufacturer: "...", quantity: 1 }
  ],
  default_shipping_address: "123 Main St, Los Angeles, CA, 90000",
  default_shipping_method: "29",
  redirect_url: "https://yoursite.example/finditparts_order_done"
}
```

`default_shipping_address` must already exist on the account — it is matched by
numerical address and zipcode. `default_shipping_method` comes from a `shippingMethods`
call for the **same** line items.

The customer sees an order summary with final cost and confirms. Completion arrives the
same two ways as step 3, with failure reported as `resolution=FAILURE/CANCELLED`.

### 6. Read the order back

`getResellerCustomerSession` on a completed `NEW_ORDER` session returns the full
`order` — `number`, `state`, `total`, `line_items`, `shipments`, `payments`. This is
your record that the order exists.

### 7. Cancelling

`cancelResellerCustomerSession` — `DELETE /reseller_customer_sessions/{id}` attempts to
cancel a submitted order. It returns `errno: 5` when the order is no longer cancelable.
Do not retry on `errno: 5`; route the customer to returns instead.

## Rules

- **Never treat a browser message as proof of an order.** Confirm with
  `getResellerCustomerSession` before telling anyone an order was placed.
- **There is no idempotency key on this API.** If a session creation call times out, do
  not blindly retry — read back with `getResellerCustomerSession` or `searchOrders`
  first, or you risk a duplicate order.
- Check `errno` on every response even at HTTP 200. `errno: 7` means the Customer
  Reference is not linked yet — you skipped or failed `USER_SETUP`. `errno: 4` means
  the line items or variant do not meet requirements. Full registry:
  `errors/finditparts-error-codes.yml`.
- Sessions carry an `expires_at`. A stale `redirect_url` will not work.
- Generic JWT (no `sub`) for reading sessions; customer-referenced JWT (`sub` set) for
  creating them. Getting this backwards yields `errno: 6`.
