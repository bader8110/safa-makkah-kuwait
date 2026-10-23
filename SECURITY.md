# Secure storefront migration — not production ready

This branch preserves the existing products, images, bilingual design and mosque supply flow. It removes the public demo management panel, coupon UI and pretend sign-in. Payments fail closed: checkout does not send data, save personal information, create a paid order or redirect to a gateway.

## Security boundary
- Never put UPayments tokens or webhook secrets in HTML, browser JavaScript, GitHub Pages, public environment variables or commits.
- A backend must load authoritative products, prices, delivery fees and stock. Browser totals are estimates only. Use integer KWD fils.
- Validate allowlisted product IDs, integer quantities, delivery coverage and bounded customer fields. Reserve stock transactionally.
- Create server-side orders with unpredictable identifiers and idempotency keys; rate limit checkout and verify request origin.
- Call UPayments from the backend and redirect only to a validated gateway URL.
- A return URL is NOT proof of payment. Validate webhook authenticity against current provider documentation, verify payment server-to-server, and match merchant, order ID, currency and amount. Process events idempotently.
- Keep personal data out of localStorage, URLs, analytics and application logs. Define retention, access controls and backups.
- Use HTTPS and host security headers: CSP including frame-ancestors, HSTS, nosniff and restrictive permissions. The included meta CSP is an interim static-preview policy, not a replacement for server headers.
- Add automated abuse tests, dependency review and independent security review before production.
- Authenticated customer accounts require a real identity service and secure HttpOnly cookies. No browser-only login.

## Operational gap
UPayments payment gateway integration does not automatically provide this independent site's catalog, inventory, fulfillment or customer account backend. Confirm a supported UStore integration with the provider, or select an established commerce backend. No custom admin dashboard is included.

## Launch gates
1. Choose the hosting/backend and order-management integration.
2. Confirm actual existing catalog and delivery rules without changing live products.
3. Configure sandbox credentials in the hosting secret store, never in this repository.
4. Test tampered totals, unknown products, duplicate payment requests/events, invalid webhooks, failed/canceled payments, stock races and tracking authorization.
5. Agree policies and WhatsApp number, verify both languages and mobile layout.
6. Enable production only after tests and security review. Rotate any previously exposed secrets.

Official API docs: https://developers.upayments.com/reference/overview
