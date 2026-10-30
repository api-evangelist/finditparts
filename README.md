# FinditParts

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

FinditParts is a Los Angeles-based online marketplace for heavy-duty truck, trailer, fleet and industrial replacement parts, selling OE, aftermarket and remanufactured components to owner-operators, fleet parts managers and mechanics nationwide. Its developer surface is the FinditParts Reseller API — a partner and reseller integration that lets shop-management platforms, e-commerce sites and fleet applications search the parts catalog with account-specific pricing, look parts up by part number and cross-reference, quote real-time shipping, create and link FinditParts customer accounts through hosted embeddable sessions, build carts and place orders on a linked customer's behalf, and read back order status and shipments.

**Areas:** Company · E-Commerce · Marketplace · Automotive · Parts · Heavy Duty Trucking · Fleet Management · Logistics · Commerce · Catalog · Order · Shipping

## FinditParts on APIs.io

Everything profiled for FinditParts is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/finditparts/](https://apis.io/providers/finditparts/)** — or go straight to one of the APIs or properties below.

### APIs (11)

- [FinditParts Addresses API](https://apis.io/apis/finditparts/finditparts-addresses-api/) — Shipping and billing address book for a user-specific session.
- [FinditParts Carts API](https://apis.io/apis/finditparts/finditparts-carts-api/) — Draft-order (cart) lifecycle: line items, addresses, shipping method, coupon, PO number and checkout.
- [FinditParts Orders API](https://apis.io/apis/finditparts/finditparts-orders-api/) — Order listing, search and detail for a user-specific session.
- [FinditParts Partners API](https://apis.io/apis/finditparts/finditparts-partners-api/) — Partner API-key surface for white-label shipping quotes and order placement.
- [FinditParts Products API](https://apis.io/apis/finditparts/finditparts-products-api/) — Part search, part-number lookup and product/variant detail across the FinditParts catalog.
- [FinditParts Reseller Customer Sessions API](https://apis.io/apis/finditparts/finditparts-reseller-customer-sessions-api/) — Hosted USER_SETUP and NEW_ORDER sessions returned as a finditparts.com redirect_url for iframe embedding or redirect.
- [FinditParts Reseller Customers API](https://apis.io/apis/finditparts/finditparts-reseller-customers-api/) — Sub-users created on a reseller master account and addressed by customer_reference.
- [FinditParts Sessions API](https://apis.io/apis/finditparts/finditparts-sessions-api/) — User-specific JWT session creation, refresh, inspection and destruction.
- [FinditParts Shipping API](https://apis.io/apis/finditparts/finditparts-shipping-api/) — Shipping method quoting and estimated cost for a proposed set of line items and an address.
- [FinditParts Users API](https://apis.io/apis/finditparts/finditparts-users-api/) — Direct FinditParts user creation for API-key clients.
- [FinditParts Variants API](https://apis.io/apis/finditparts/finditparts-variants-api/) — Variant-level price, quantity, cutoff and core lookups.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [finditparts-mcp.yml](https://apis.io/servers/finditparts/finditparts-mcp-yml/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Finditparts Agentic Access](https://apis.io/agentic-access/finditparts/finditparts-agentic-access/)

#### Examples (38)

Example requests and responses.

- [Finditparts Add Cart Line Item 200](https://apis.io/examples/finditparts/finditparts-add-cart-line-item-200/)
- [Finditparts Cancel Reseller Customer Session 200](https://apis.io/examples/finditparts/finditparts-cancel-reseller-customer-session-200/)
- [Finditparts Change Cart Line Items 200](https://apis.io/examples/finditparts/finditparts-change-cart-line-items-200/)
- [Finditparts Complete Cart With Corporate Billing 200](https://apis.io/examples/finditparts/finditparts-complete-cart-with-corporate-billing-200/)
- [Finditparts Complete Cart With Credit Card 200](https://apis.io/examples/finditparts/finditparts-complete-cart-with-credit-card-200/)
- [Finditparts Create Address 200](https://apis.io/examples/finditparts/finditparts-create-address-200/)
- [Finditparts Create Cart 200](https://apis.io/examples/finditparts/finditparts-create-cart-200/)
- [Finditparts Create Reseller Customer 200](https://apis.io/examples/finditparts/finditparts-create-reseller-customer-200/)
- [Finditparts Create Reseller Customer Session 200](https://apis.io/examples/finditparts/finditparts-create-reseller-customer-session-200/)
- [Finditparts Create Session 200](https://apis.io/examples/finditparts/finditparts-create-session-200/)
- [Finditparts Create User 200](https://apis.io/examples/finditparts/finditparts-create-user-200/)
- [Finditparts Delete Address 200](https://apis.io/examples/finditparts/finditparts-delete-address-200/)
- [Finditparts Destroy Sessions 200](https://apis.io/examples/finditparts/finditparts-destroy-sessions-200/)
- [Finditparts Get Cart 200](https://apis.io/examples/finditparts/finditparts-get-cart-200/)
- [Finditparts Get Cart Shipping Methods 200](https://apis.io/examples/finditparts/finditparts-get-cart-shipping-methods-200/)
- [Finditparts Get Current Session 200](https://apis.io/examples/finditparts/finditparts-get-current-session-200/)
- [Finditparts Get Order 200](https://apis.io/examples/finditparts/finditparts-get-order-200/)
- [Finditparts Get Product 200](https://apis.io/examples/finditparts/finditparts-get-product-200/)
- [Finditparts Get Products Multi 200](https://apis.io/examples/finditparts/finditparts-get-products-multi-200/)
- [Finditparts Get Reseller Customer Session 200](https://apis.io/examples/finditparts/finditparts-get-reseller-customer-session-200/)
- [Finditparts List Addresses 200](https://apis.io/examples/finditparts/finditparts-list-addresses-200/)
- [Finditparts List Orders 200](https://apis.io/examples/finditparts/finditparts-list-orders-200/)
- [Finditparts List Reseller Customers 200](https://apis.io/examples/finditparts/finditparts-list-reseller-customers-200/)
- [Finditparts Lookup Product By Part Number 200](https://apis.io/examples/finditparts/finditparts-lookup-product-by-part-number-200/)
- [Finditparts Partners Place Order 200](https://apis.io/examples/finditparts/finditparts-partners-place-order-200/)
- …and 13 more published on APIs.io

#### Collections (1)

Postman and Open Collection request collections.

- [FinditParts Reseller API](https://apis.io/collections/finditparts/open-finditparts-reseller-api/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Finditparts Reseller Api Overlay](https://apis.io/overlays/finditparts/finditparts-reseller-api-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Finditparts Authentication](https://apis.io/security/finditparts/finditparts-authentication/)
- [Finditparts Domain Security](https://apis.io/security/finditparts/finditparts-domain-security/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Finditparts Plans Pricing](https://apis.io/plans/finditparts/finditparts-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Finditparts Rate Limits](https://apis.io/rate-limits/finditparts/finditparts-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.finditparts.com/)
- [Documentation](https://api-docs.finditparts.com/)
- [API reference](https://api-docs.finditparts.com/)
- [Sign up](https://www.finditparts.com/signup)
- [Support](https://www.finditparts.com/support)
- [Blog](https://www.finditparts.com/blog)
- [Terms of service](https://www.finditparts.com/terms-of-use)
- [Privacy policy](https://www.finditparts.com/privacy)
- [GitHub](https://github.com/FinditParts)

## API Evangelist rating

**Kin Score 39.9** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.2 |
| Contract quality | 55.3 |
| Contract governance | 4.5 |
| Operational transparency | 2.6 |
| Developer ergonomics | 51.8 |
| Access clarity | 34.2 |

**Agent readiness 31.8** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | documented (partial credit) |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | verified |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | distinct |

**Regulatory alignment 24.5** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/finditparts/](https://apis.io/providers/finditparts/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for FinditParts: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/finditparts/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
