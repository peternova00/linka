# DataRelay — local data bundle site

This project is configured for local testing. It serves the Ghana data-bundle storefront and keeps the existing bundle checkout plus Site A payment-service routes available.

## Run locally

```bash
cp .env.example .env
npm test
npm run dev
```

Open `http://localhost:4000` in your browser.

The local defaults are:

```text
Data bundle site: http://localhost:4000
Site A callback peer: http://localhost:3000
Paystack API: https://api.paystack.co using a test key
```

The Paystack API remains external because it handles payment checkout and verification. Use a Paystack test key for local work; never put the key in `public/app.js`.

## Payment routes kept for local testing

Bundle checkout:

```text
POST /api/v1/data-bundles/initialize
POST /api/v1/data-bundles/:reference/complete
GET  /api/v1/data-bundles/orders/:reference
```

Site A payment-service initialization remains available locally at:

```text
POST /api/v1/payments/initialize
```

It still expects the existing signed request contract. Set `SITE_A_TO_SITE_B_SECRET` and `SITE_B_TO_SITE_A_SECRET` to matching local test values when running a local Site A on port 3000.

## Manual fulfillment

After a verified Paystack payment, a bundle order is stored as `PAID_AWAITING_MANUAL_DELIVERY`. The private manual-order API is:

```text
GET  /api/v1/data-bundles/admin/orders
POST /api/v1/data-bundles/admin/orders/:reference/fulfill
Authorization: Bearer $DATA_BUNDLE_ADMIN_TOKEN
```
