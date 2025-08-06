
# How to Build a Checkout Page with Airwallex

This guide outlines the step-by-step instructions to build a checkout page integrated with Airwallex APIs and Airwallex.js.

---

## Step 1: API Authentication

To start using Airwallex APIs, authenticate with your demo credentials to obtain an access token.

1. Go to [Airwallex Sandbox](https://demo.airwallex.com/signup/sg/sandbox) and register or log in.
2. Navigate to **Developer → API Keys** to get your **client-id** and **api-key**.
3. Exchange them for an access token using the following request:

```bash
curl --request POST \
--url 'https://api-demo.airwallex.com/api/v1/authentication/login' \
--header 'Content-Type: application/json' \
--header 'x-client-id: {CLIENT_ID}' \
--header 'x-api-key: {API_KEY}'
```

A successful response returns an access token:

```json
{
  "expires_at": "2025-08-05T10:53:01+0000",
  "token": "{ACCESS_TOKEN}"
}
```

Use this token in the `Authorization` header for all subsequent API calls:
```
Authorization: Bearer {ACCESS_TOKEN}
```

---

## Step 2: Create a Payment Intent

Create a Payment Intent to initiate the payment flow.

```bash
curl --request POST \
--url 'https://api-demo.airwallex.com/api/v1/pa/payment_intents/create' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'Content-Type: application/json' \
--data '{
  "amount": 100,
  "currency": "USD",
  "merchant_order_id": "ORDER12345",
  "request_id": "UUID-GOES-HERE",
  "descriptor": "Demo Purchase",
  "return_url": "https://yourdomain.com/return"
}'
```

Save the returned `id` and `client_secret` from the response. These are needed to embed the payment UI.

---

## Step 3: Install and Initialize Airwallex.js

Install Airwallex.js in your frontend application.

Using NPM:
```bash
npm install @airwallex/components-sdk
```

Or include via CDN in your HTML:
```html
<script src="https://checkout.airwallex.com/assets/elements.bundle.min.js"></script>
```

Initialize it:

```ts
import { init } from '@airwallex/components-sdk';

await init({
  locale: 'en',
  env: 'demo',
  enabledElements: ['payments']
});
```

---

## Step 4: Embed the Drop-in Element

Use the client_secret and intent_id to display a pre-built checkout UI.

```ts
import { createElement } from '@airwallex/components-sdk';

const element = await createElement('dropIn', {
  intent_id: 'replace-with-your-intent-id',
  client_secret: 'replace-with-your-client-secret',
  currency: 'USD'
});

element.mount('checkout-container');
```

In your HTML:

```html
<div id="checkout-container"></div>
```

You can also listen for events:

```ts
element.on('success', (e) => {
  const { intent } = e.detail;
  // Handle post-payment logic
});
```

---

## Step 5: Retrieve a Payment Intent (Optional)

To check payment status:

```bash
curl --request GET \
--url 'https://api-demo.airwallex.com/api/v1/pa/payment_intents/{id}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'Content-Type: application/json'
```

Response:

```json
{
  "id": "int_xxx",
  "status": "SUCCEEDED",
  ...
}
```

---

## Summary

You now have:

- Authenticated to Airwallex API
- Created a Payment Intent
- Integrated the Drop-in Element via Airwallex.js
- Retrieved Payment status via API

You’re ready to build and launch your checkout with Airwallex!
