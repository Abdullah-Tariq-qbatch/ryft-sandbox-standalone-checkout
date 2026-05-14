# ryft-sandbox-standalone-checkout

A zero-dependency, single-file HTML checkout page powered by the [Ryft Embedded SDK](https://developer.ryftpay.com/documentation/get_started/process_payments/embedded_sdk/initial_setup). Open it directly in any browser — no server, no build step, no npm install.

---

## What it does

- Renders a fully functional Ryft card payment form in the browser
- Accepts a **Client Secret**, **Public Key**, and optional **Account ID** via an on-page form
- Handles payment submission, success states, and user-facing errors
- Supports standard account payments and sub-account / platform fee payments

---

## Usage

#### Optional
Deployed Page: [https://abdullah-tariq-qbatch.github.io/ryft-sandbox-standalone-checkout/](https://abdullah-tariq-qbatch.github.io/ryft-sandbox-standalone-checkout/)

### 1. Download the file

```
ryft-checkout.html
```

### 2. Open in a browser

Double-click the file, or open via `File → Open` in any modern browser.

### 3. Fill in the config

| Field | Where to get it | Required |
|---|---|---|
| **Client Secret** | Returned by your backend when creating a Payment Session (`ps_…_secret_…`) | ✅ |
| **Public Key** | Ryft Portal → API Keys (`pk_sandbox_…` or `pk_live_…`) | ✅ |
| **Account ID** | Sub-account ID (`ac_…`) — only needed for Platform Fee / split payment models | Optional |

Click **Apply & Load Payment Form**, then enter card details and click **Pay Now**.

---

## URL Parameters (shortcut)

Skip manual input by passing values as query parameters. The form will auto-fill and initialise on load:

```
ryft-checkout.html?clientSecret=ps_...&publicKey=pk_...
```

```
ryft-checkout.html?clientSecret=ps_...&publicKey=pk_...&accountId=ac_...
```

Useful for sharing pre-configured links with teammates or embedding in internal tooling.

---

## Payment Session

The Client Secret comes from a Payment Session created server-side. Quick example:

```bash
curl -X POST https://sandbox-api.ryftpay.com/v1/payment-sessions \
  -H "Authorization: YOUR_SECRET_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "amount": 1000,
    "currency": "GBP",
    "customerEmail": "customer@example.com",
    "returnUrl": "https://yoursite.com/order-complete"
  }'
```

The response includes a `clientSecret` — paste that value into the checkout page.

---

## Test Cards

Use these in sandbox mode (`pk_sandbox_…`):

| Card Number | Scenario |
|---|---|
| `4929 0000 0000 6` | Payment approved |
| `4000 0000 0000 0002` | Card declined |
| `4000 0000 0000 3220` | 3DS required |

Use any future expiry, any 3-digit CVV, and any cardholder name.

Full list: [Ryft Test Cards](https://developer.ryftpay.com/documentation/get_started/test_cards)

---

## SDK Version

Uses **Ryft Embedded SDK v2** loaded via CDN:

```html
<script src="https://embedded.ryftpay.com/v2/ryft.min.js"></script>
```

---

## Notes

- This file is intended for **testing and internal use**. For production checkouts, integrate the SDK into your frontend application.
- Never expose your **Secret Key** (`sk_…`) in a browser. Only the Public Key (`pk_…`) and Client Secret (`ps_…_secret_…`) are safe to use client-side.
- The Client Secret is single-use and tied to one Payment Session.

---

## Resources

- [Ryft Developer Docs](https://developer.ryftpay.com)
- [Embedded SDK Setup](https://developer.ryftpay.com/documentation/get_started/process_payments/embedded_sdk/initial_setup)
- [Ryft Portal](https://portal.ryftpay.com)
