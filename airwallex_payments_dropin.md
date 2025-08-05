# Airwallex API Instructions

## API Authentication
The user need to provide their demo admin **client-id** and **api-key** to exchange the access token for API calls, which can be generated in the "Developer -> API Keys" page once registered/logged into the Airwallex [Sandbox](https://demo.airwallex.com/signup/sg/sandbox).

To exchange the access token, POST HTTP request to https://api-demo.airwallex.com/api/v1/authentication/login with the headers 'x-client-id' and 'x-api-key' assigned the corresponding values. For example
```bash
url --request POST \
--url 'https://api-demo.airwallex.com/api/v1/authentication/login' \
--header 'Content-Type: application/json' \
--header 'x-client-id: {CLIENT_ID}' \
--header 'x-api-key: {API_KEY}'
```
The `token` will be returned in the response, which can be resued in subsequent API requests (by passing it in the header `Authorization: Bearer {ACCESS_TOKEN}`).
```json
{
  "expires_at": "2025-08-05T10:53:01+0000",
  "token": "{ACCESS_TOKEN}"
}
```
## Create a Payment Intent
Start collecting payment from your customer by creating a Payment Intent. The payment `amount` and `currency`, as well as the `merchant_order_id` must be provided.
For example
```bash
curl --request POST \
--url 'https://api-demo.airwallex.com/api/v1/pa/payment_intents/create' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'Content-Type: application/json' \
--data '{
  "amount": 100,
  "currency": "USD",
  "merchant_order_id": "D202503210001",
  "request_id": "b01737e5-c5ab-4765-8834-cbd92dfeaf81",
  "descriptor": "Airwallex - Test Descriptor",
  "return_url": "https://www.airwallex.com"
}'
```
Returns
```json
{
  "id": "int_hkpdskz7vg1xc7uscdj",
  "request_id": "b01737e5-c5ab-4765-8834-cbd92dfeaf81",
  "amount": 100,
  "currency": "USD",
  "status": "REQUIRES_PAYMENT_METHOD",
  "merchant_order_id": "D202503210001",
  "return_url": "https://www.airwallex.com",
  "descriptor": "Airwallex - Test Descriptor",
  "client_secret": "{CLIENT_SECRET}",
  "created_at": "2025-01-31T06:57:10+0000",
  "updated_at": "2025-01-31T06:57:10+0000"
}
```

**Request Details**
- `amount` (number, required)
  Payment amount. This is the order amount you would like to charge your customer.

- `currency` (string, required)
  Payment currency in 3-letter ISO 4217 currency code.

- `merchant_order_id` (string, required)
  The order ID created in merchant's order system that corresponds to this PaymentIntent. Maximum length is 64.

- `request_id` (string, required)
  Unique request ID specified by the merchant. Maximum length is 64.

- `return_url` (string, optional)
  The web page URL or application scheme URI to redirect the customer back after payment authentication.

- `descriptor` (string, optional)
  Descriptor that will display to the customer. Maximum length is 32.

- `order` (object, optional)
  Purchase order of this Payment Intent.
  - `products` (array of object, optional)
    - `desc` (string, optional)
      Product description. Maximum of 500 characters.
    - `name` (string, optional)
      Name of the product. Maximum of 255 characters.
    - `quantity` (integer, optional)
      Product description. Maximum of 500 characters.
    - `sku` (string, optional)
      Product stock keeping unit. Maximum of 128 characters.
    - `unit_price` (number, optional)
      Product unit price

**Response Details**
- `id` (string)
  Unique identifier for the PaymentIntent

- `amount` (number)
  Payment amount. This is the order amount you would like to charge your customer.

- `currency` (string)
  Payment currency in 3-letter ISO 4217 currency code.

- `merchant_order_id` (string)
  The order ID created in merchant's order system that corresponds to this PaymentIntent.

- `request_id` (string)
  Unique request ID specified by the merchant.

- `return_url` (string)
  The web page URL or application scheme URI to redirect the customer back after payment authentication.

- `descriptor` (string)
  Descriptor that will display to the customer.

- `order` (object)
  Purchase order of this Payment Intent.
    - `products` (array of object)
        - `desc` (string)
          Product description.
        - `name` (string)
          Name of the product.
        - `quantity` (integer)
          Product description.
        - `sku` (string)
          Product stock keeping unit.
        - `unit_price` (number)
          Product unit price

- `client_secret` (string)
  The client secret of this Payment Intent for browser or app, valid for 60 minutes.

- `status` (string)
  The status of this Payment Intent. Possible values:
  - `REQUIRES_PAYMENT_METHOD` - The PaymentIntent is waiting for the confirm request. This status is returned right after the PaymentIntent is created or the previous PaymentAttempt has failed or expired.
  - `REQUIRES_CUSTOMER_ACTION` - The PaymentIntent is waiting for further customer action of authentication, e.g. 3DS verification and QR code scan. Please check the next_action.
  - `REQUIRES_CAPTURE` - The PaymentIntent is waiting for your capture to complete the payment.
  - `PENDING` - The PaymentIntent is pending the final result from the provider. No further action is required.
  - `SUCCEEDED` - The PaymentIntent has succeeded. The payment is complete.
  - `CANCELLED` - The PaymentIntent has been canceled by your request. The payment is closed.

- `created_at` (string)
  Time at which this PaymentIntent was created

- `updated_at` (string)
  Last time at which this PaymentIntent was updated or operated on

## Retrieve a Payment Intent
Retrieve a PaymentIntent by ID.
```bash
curl --request GET \
--url 'https://api-demo.airwallex.com/api/v1/pa/payment_intents/{id}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'Content-Type: application/json'
```
Returns
```json
{
  "id": "int_hkpdskz7vg1xc7uscdj",
  "request_id": "b01737e5-c5ab-4765-8834-cbd92dfeaf81",
  "amount": 100,
  "currency": "USD",
  "status": "SUCCEEDED",
  "merchant_order_id": "D202503210001",
  "return_url": "https://www.airwallex.com",
  "descriptor": "Airwallex - Test Descriptor",
  "created_at": "2025-01-31T06:57:10+0000",
  "updated_at": "2025-01-31T06:57:10+0000"
}
```

**Request URL Parameters**
- `id` (number, required)
  Unique identifier for the PaymentIntent

**Response Details**
- `id` (string)
  Unique identifier for the PaymentIntent

- `amount` (number)
  Payment amount. This is the order amount you would like to charge your customer.

- `currency` (string)
  Payment currency in 3-letter ISO 4217 currency code.

- `merchant_order_id` (string)
  The order ID created in merchant's order system that corresponds to this PaymentIntent.

- `request_id` (string)
  Unique request ID specified by the merchant.

- `return_url` (string)
  The web page URL or application scheme URI to redirect the customer back after payment authentication.

- `descriptor` (string)
  Descriptor that will display to the customer.

- `order` (object)
  Purchase order of this Payment Intent.
    - `products` (array of object)
        - `desc` (string)
          Product description.
        - `name` (string)
          Name of the product.
        - `quantity` (integer)
          Product description.
        - `sku` (string)
          Product stock keeping unit.
        - `unit_price` (number)
          Product unit price

- `client_secret` (string)
  The client secret of this Payment Intent for browser or app, valid for 60 minutes.

- `status` (string)
  The status of this Payment Intent. Possible values:
    - `REQUIRES_PAYMENT_METHOD` - The PaymentIntent is waiting for the confirm request. This status is returned right after the PaymentIntent is created or the previous PaymentAttempt has failed or expired.
    - `REQUIRES_CUSTOMER_ACTION` - The PaymentIntent is waiting for further customer action of authentication, e.g. 3DS verification and QR code scan. Please check the next_action.
    - `REQUIRES_CAPTURE` - The PaymentIntent is waiting for your capture to complete the payment.
    - `PENDING` - The PaymentIntent is pending the final result from the provider. No further action is required.
    - `SUCCEEDED` - The PaymentIntent has succeeded. The payment is complete.
    - `CANCELLED` - The PaymentIntent has been canceled by your request. The payment is closed.

- `created_at` (string)
  Time at which this PaymentIntent was created

- `updated_at` (string)
  Last time at which this PaymentIntent was updated or operated on

# Airwallex JS Instructions
Airwallex.js is a browser-side JavaScript library that lets you seamlessly embed pre-built UI elements into your web pages. With Airwallex.js, you can enable payment acceptance, customer onboarding (KYC/KYB), payouts, risk management, and more—all with minimal effort.

This reference provides detailed documentation on every available element, object, and method in Airwallex.js.

## Install Airwallex.js
Install Airwallex.js using your preferred package manager, for example, npm, yarn, or pnpm.
Alternatively with cdn installation, you can add Airwallex.js as a HTML `<script>` tag to the `<head>` element of each HTML page on your site. This installation option allows any newly created Airwallex.js objects to be globally accessible in your client side code.

## Initialize Airwallex.js
Load Airwallex.js and specify the initialization settings and elements you want to fetch using the `init()` function. The modular approach of fetching only the resources specified in `init()` ensures optimized performance and customization tailored to your needs.
> The Airwallex package should only be initialized once in your application.

### init(options?)
**Parameters**
- `options`  (object, optional)
  - `env` (string, optional)
    The Airwallex environment you want to integrate your application with. One of `demo`, `prod`. Default value is `prod`.
  - `fonts` (object, optional)
    Fonts options to customize the font styles of Payment Elements.
    - `family` (string, optional)
      The font-family property, for example, `AxLLCircular`. https://developer.mozilla.org/en-US/docs/Web/CSS/font-family
    - `src` (string, optional)
      The font-source property, for example, `https://checkout.airwallex.com/fonts/CircularXXWeb/CircularXXWeb-Regular.woff2`. https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face
    - `weight` (number, optional)
      The font-weight property. Supported font weights include: regular (400) and bold (700). https://developer.mozilla.org/en-US/docs/Web/CSS/font-weight
  - `enabledElements` (array of string, optional)
    An array of Element groups representing the Elements. Possible Element names: `payments`, `payouts`, `onboarding`, `risk`. For example, `payments` indicates all available Elements that support collection of payments information.
  
**Returns**
- void

**Example**
```typescript
// Example 1: Initialize Airwallex.js for Payment Elements
import { init } from '@airwallex/components-sdk';
const options = {
  locale: 'en',
  env: 'prod',
  enabledElements: ['payments'],
};
await init(options);

// Example 2: Initialize Airwallex.js for Onboarding, Payout, Risk, or Compliance Support Elements
import { init } from '@airwallex/components-sdk';
const options = {
  locale: 'en',
  env: 'prod',
  enabledElements: ['onboarding', 'payouts', 'risk'],
  authCode: 'replace-with-your-auth-code',
  clientId: 'replace-with-your-client-id',
  codeVerifier: 'replace-with-your-code-verifier',
};
await init(options);
```

## Drop-in Element
The Drop-in Element lets you embed multiple payment methods with a single integration while offering options to customize the look and feel of the checkout experience.

### createElement('dropIn', options?)
Use this function to create an instance of an individual Element.

**Parameters**
- `type`  (string, optional)
  The type of element you are creating. Must be `dropIn`.
- `options` (object, optional)
  Options for creating dropIn Element.
  - `client_secret` (string, required)
    The client_secret of the Payment Intent when Payment Intent is provided. Otherwise, this should be the client_secret of the Customer object.
  - `currency` (string, required)
    The three-letter ISO currency code representing the currency of the Payment Intent or Payment Consent. Only payment methods supported for the specified currency will be shown on the payment page.
  - `intent_id` (string, optional)
    The ID of the Payment Intent you would like to checkout. Required when mode is `payment`.
  - `customer_id` (string, optional)
    The ID of the Customer used in registered user checkout. This field is required when mode is 'recurring'.
  - `mode` (string, optional)
    The checkout mode for the shopper, either `payment` or `recurring`. Default value is `payment`.
  - `country_code` (string, optional)
    The two-letter ISO country code of the shopper's country. This is required for bank_transfer, online_banking, skrill or seven_eleven payment methods.
  - `layout` (object, optional)
    The layout of the DropIn element.
    - `alwaysShowMethodLabel` (boolean, optional)
      By default, the payment method icon is not visible when there is only one payment method. Set it to `true` if you want to always show the payment method icon. Default value is `false`.
    - `type` (string, optional)
      Specify the layout for the payment elements. By default, `accordion` layout is used on desktop and `tab` layout is used on mobile.
  - `autoCapture` (boolean, optional)
    Whether the amount should be captured automatically upon successful payment authorization. Set it to `false` if you want to place a hold on the payment method and capture the funds sometime later. Default value is `true`.
  - `shopper_name` (string, optional)
    The shopper's full name. If provided, some payment method will skip requiring shopper to input it.
  - `shopper_email` (string, optional)
    The shopper's email. If provided, some payment method will skip requiring shopper to input it.
  - `shopper_phone` (string, optional)
    The shopper's phone number. If provided, some payment method will skip requiring shopper to input it.

**Example**
```typescript
import { createElement } from '@airwallex/components-sdk';
const element = await createElement('dropIn', {
  intent_id: 'replace-with-your-intent-id',
  client_secret: 'replace-with-your-client-secret',
  currency: 'replace-with-your-intent-currency',
});
```

### destroy()
Destroys the Element instance.

**Returns**
- void

### mount(domElement)
Mounts Element to your HTML DOM.

**Parameters**
- `domElement` (string, required)

**Returns**
- HTMLElement or null

**Example**
```typescript
// There are two ways to mount the element:
// 1. Call with the container DOM id
element.mount('container-dom-id');

// 2. Find the created DOM in the existing HTML and call with the container DOM element
const containerElement = document.getElementById('container-dom-id');
element.mount(containerElement);
```

### unmount()
Unmounts the Element. Note that the Element instance will remain.

**Returns**
- void

**Example**
```typescript
element.unmount();
```

### on(eventCode, handler)
Listen to the Element events.

**Parameters**
- `eventCode` (string, required)
  The event code to listen for. One of `ready`, `success`, `error`, `cancel`, `clickConfirmButton`, `switchMethod`, `pendingVerifyAccount`.
- `handler` (function, required)
  The callback function that will be called when the event occurs.

**Returns**
- void

**Example**
```typescript
element.on('success', (e) => {
  const { intent } = e.detail;
 // Handle success event
});
```

# How to build a checkout page with Airwallex API and Drop-In Element
## Step 1 - Create a Payment Intent by API from your server
When the shopper begins the checkout process, you will need to create a Payment Intent to indicate your intent to collect payment from the shopper.

When the checkout page loads, on your server, call `Create a PaymentIntent` with an amount and currency. Always decide how much to charge on the server side, a trusted environment, as opposed to the client. This prevents malicious shoppers from being able to alter the payment amount.

Provide `return_url` in `Create a PaymentIntent` if you want to offer redirection-based payment methods (Alipay, Dana, KakaoPay, etc) that redirect shoppers to a partner site. The shopper will be returned to the `return_url` after the payment is complete, whether successful or otherwise.

The PaymentIntent’s `id` and `client_secret` are returned in the response — these parameters let you confirm the payment and update card details on the client, without allowing manipulation of sensitive information, like payment amount.

## Step 2 - Add Drop-In Element to your checkout page
First, you will need to import Airwallex.js and then initialize the package.

To embed the Drop-in Element  into your checkout page, create an empty container `div` with a unique id in your payment form. Ensure that the payment form only contains one Element with this unique id. Airwallex inserts an iframe into this `div` on mounting the Element.
```HTML
<div id="dropIn"></div>
```
When the payment form has loaded, call `createElement(type, options)` by specifying the Element type as `dropIn` to create the Element. Ensure that the payment form only contains one Element with `dropIn` id.
```javascript
import { createElement } from '@airwallex/components-sdk';

const element = createElement('dropIn', {
  intent_id: 'replace-with-your-intent-id',
  client_secret: 'replace-with-your-client-secret',
  currency: 'replace-with-your-intent-currency',
});
```
Call `mount()`  with the id of the `div` to mount the Element to the DOM. This builds a payment form with various payment methods. The Element should only be mounted once in a single payment flow.
```javascript
element.mount('dropIn'); // Injects iframe into the Drop-in Element container
```
When the Drop-in Element is mounted a `ready` event is triggered. Listen this event to prepare and load the checkout page.
```javascript
element.on('ready', (event) => {
// Handle event on ready
});
```
## Step 3 - Handle the response
Add `success` and `error` event listeners to handle error and success events received from Airwallex.
```javascript
const element = mount('dropIn');
element.on('success', (event) => {
// Handle event on success
});

element.on('error', (event) => {
// Handle event on error
});
```
If no error occurred, display a message that the payment was successful. If payment fails with an error, display the appropriate message to your shopper so they can take action and try again.
## Step 4 - Retrieve the payment result
Some payment method might redirect the shopper to a provider hosted page. Once the shopper completes or fails the payment, the shopper will be redirected back to the `return_url`, which should be the payment result page of the order. Your server should loop-query the `Retrieve a Payment Intent` API several times for the final status of the corresponding Payment Intent. 