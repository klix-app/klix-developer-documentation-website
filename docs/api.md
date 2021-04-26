# Klix API

Klix provides unified API for multiple payment methods - card and PSD2 bank payments. Full up-to-date REST API Reference can be found [here](https://portal.klix.app/api). OpenAPI document that can be used to generate API client can be found [here](https://portal.klix.app/api/schema/v1/).
In order to call Klix API you should first receive API credentials (Brand ID and Secret Key) from Klix representative.

## API usage in single payment scenario

### Single payment step by step guide

1. Get list of payment methods (optional). This API end-point returns name and logo of each payment method available to merchant (Klix Card payments, Citadele and other bank PSD2 payments etc.). Use this list to render a payment methods page on your site.
2. Create a purchase by submitting order data to Klix. Once purchase is created link to Klix hosted payment page will be returned as `checkout_url` field value. After customer is redirected to this page Klix will handle a payment process.
3. Handle one of possible payment process outcomes:
    * If customer payment is successful then callback is sent to merchant server and customer is redirected to successful purchase page specified by merchant. Note that you should consider purchase as successfully paid only after callback is received and purchase status is checked. Note that in case if callback is not used you can call Klix API to get purchase status once customer is redirected back to successful purchase page.
    * If customer payment fails for some reason customer is redirected to failed purchase page.
    * If cancelled payment redirect URL is specified in purchase creation request then customer will be able to go back to merchant page from Klix payment page. It's preferable to specify checkout/payment method selection page as cancelled payment redirect URL so that customer can make adjustments in shopping cart or choose different payment method.

### Single payment request examples

These are simple request examples that illustrate Klix API usage. Always use [API Reference](https://portal.klix.app/api) as a single source of truth.
Note that `<Brand ID goes here>` and `<Secret key goes here>` should be replaced with actual `Brand ID` and `Secret Key` received from Klix contact person.

#### Get list of available payment methods

```sh
curl -X GET \
  'https://portal.klix.app/api/v1/payment_methods/?currency=EUR&brand_id=<Brand ID goes here>' \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer <Secret key goes here>' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Host: portal.klix.app' \
  -H 'accept-encoding: gzip, deflate' \
  -H 'cache-control: no-cache'
```

#### Create available payment methods purchase

This option allows to create a purchase that can be paid using any payment method available to merchant. Customer will be have an option to choose one of available payment methods after redirect to Klix payment page.

```sh
curl -X POST \
  https://portal.klix.app/api/v1/purchases/ \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer <Secret key goes here>' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Content-Type: application/json' \
  -H 'Host: portal.klix.app' \
  -H 'accept-encoding: gzip, deflate' \
  -H 'cache-control: no-cache' \
  -d '{
   "success_callback": "https://your.site/api/successfully-paid-callback-will-be-sent-to-this-end-point",
   "success_redirect": "https://your.site/customer-will-be-redirected-here-in-case-of-successfull-payment",
   "failure_redirect": "https://your.site/customer-will-be-redirected-here-in-case-of-failed-payment",
   "cancel_redirect": "https://your.site/customer-will-be-redirected-here-in-case-customer-decides-to-go-back-to-your-store-during-payment",
   "purchase":{
      "language": "lv",
      "products":[
         {
            "price":3000,
            "name":"Xiaomi Mi Smart Band 5"
         },
         {
            "price":100,
            "name":"Screen protector for Xiaomi Mi Smart Band 5"
         }
      ]
   },
   "client":{
      "email":"test@test.com"
   },
   "brand_id":"<Brand ID goes here>",
   "reference": "Your order id"
}'
```

#### Create whitelisted payment method purchase

This option allows to create a purchase that can be paid only using specified whitelisted payment method. Most often this option is used to allow purchase to be paid using payment method selected on merchant web-site.

```sh
curl -X POST \
  https://portal.klix.app/api/v1/purchases/ \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer <Secret key goes here>' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Content-Type: application/json' \
  -H 'Host: portal.klix.app' \
  -H 'accept-encoding: gzip, deflate' \
  -H 'cache-control: no-cache' \
  -d '{
   "success_callback": "https://your.site/api/successfully-paid-callback-will-be-sent-to-this-end-point",
   "success_redirect": "https://your.site/customer-will-be-redirected-here-in-case-of-successfull-payment",
   "failure_redirect": "https://your.site/customer-will-be-redirected-here-in-case-of-failed-payment",
   "cancel_redirect": "https://your.site/customer-will-be-redirected-here-in-case-customer-decides-to-go-back-to-your-store-during-payment",
   "purchase":{
      "language": "lv",
      "products":[
         {
            "price":3000,
            "name":"Xiaomi Mi Smart Band 5"
         },
         {
            "price":100,
            "name":"Screen protector for Xiaomi Mi Smart Band 5"
         }
      ]
   },
   "client":{
      "email":"test@test.com"
   },
   "payment_method_whitelist": ["klix"],
   "brand_id":"<Brand ID goes here>",
   "reference": "Your order id"
}'
```

#### Get purchase information

`<Purchase ID>` is purchase identifier (field `id` value) received in purchase creation response.

```sh
curl -X GET \
  https://portal.klix.app/api/v1/purchases/<Purchase ID>/ \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer <Secret key goes here>' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Host: portal.klix.app' \
  -H 'accept-encoding: gzip, deflate' \
  -H 'cache-control: no-cache'
```

## API usage in recurring payment scenario

### Recurring payment step by step guide

1. Create an initial purchase with recurring flag set to `true` (see `"force_recurring": true` in [example](#create-an-initial-purchase-to-register-card-for-subsequent-recurring-payments)) in order to register payment card details for subsequent use in recurring payments. Created purchase `id` and `checkout_url` will be returned in response. Redirect customer to `checkout_url` and Klix will handle a payment process. Store this initial purchase `id` on your side as a recurring payment token and use it later on to charge a customer payment card (see `<Recurring payment token>` placeholder in [example](#charge-customer-card-as-a-recurring-payment)). If purchase created in previous step is paid by customer then card details are successfully captured for subsequent use.
2. Once customer card should be charged in a form of a recurring payment new purchase should be created via API by specifying amount that needs to be charged and transaction description. This time `force_recurring` should be `false` or can be omitted at all (see [example](#create-recurring-payment)). In this case only purchase `id` returned in response should be used in next step (see `<Purchase id of recurring payment>` placeholder in [example](#charge-customer-card-as-a-recurring-payment)). Note that `checkout_url` should not be used.
3. Created purchase should be charged via API call. Note that both newly created purchase `id` created in step 2 and recurring payment token (purchase `id`) created in step 1 should be passed to this API end-point (see [example](#charge-customer-card-as-a-recurring-payment)).

### Request examples

These are simple request examples that illustrate Klix API usage. Always use [API Reference](https://portal.klix.app/api) as a single source of truth.
Note that `<Brand ID goes here>` and `<Secret key goes here>` should be replaced with actual `Brand ID` and `Secret Key` received from Klix contact person.

#### Create an initial purchase to register card for subsequent recurring payments

This should be done only once to make initial payment and register customer card for subsequent use.

```sh
curl -X POST \
  https://portal.klix.app/api/v1/purchases/ \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer <Secret key goes here>' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Content-Type: application/json' \
  -H 'Host: portal.klix.app' \
  -H 'accept-encoding: gzip, deflate' \
  -H 'cache-control: no-cache' \
  -d '{
   "force_recurring": true,
   "success_callback": "https://your.site/api/successfully-paid-callback-will-be-sent-to-this-end-point",
   "success_redirect": "https://your.site/customer-will-be-redirected-here-in-case-of-successfull-payment",
   "failure_redirect": "https://your.site/customer-will-be-redirected-here-in-case-of-failed-payment",
   "cancel_redirect": "https://your.site/customer-will-be-redirected-here-in-case-customer-decides-to-go-back-to-your-store-during-payment",
   "purchase":{
      "language": "lv",
      "products":[
         {
            "price":700,
            "name":"XYZ service subscription"
         }
      ]
   },
   "client":{
      "email":"test@test.com"
   },
   "brand_id":"<Brand ID goes here>",
   "reference": "Your order id"
}'
```

#### Create recurring payment

This should be done for each recurring payment (e.g. each month).

```sh
curl -X POST \
  https://portal.klix.app/api/v1/purchases/ \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer <Secret key goes here>' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Content-Type: application/json' \
  -H 'Host: portal.klix.app' \
  -H 'accept-encoding: gzip, deflate' \
  -H 'cache-control: no-cache' \
  -d '{
   "success_callback": "https://your.site/api/successfully-paid-callback-will-be-sent-to-this-end-point",
   "purchase":{
      "language": "lv",
      "products":[
         {
            "price":700,
            "name":"XYZ service subscription fee for December 2020"
         }
      ]
   },
   "client":{
      "email":"test@test.com"
   },
   "brand_id":"<Brand ID goes here>",
   "reference": "Your order id"
}'
```

#### Charge customer card as a recurring payment

```sh
curl -X POST \
  https://portal.klix.app/api/v1/purchases/<Purchase id of recurring payment>/charge \
  -H 'Accept: application/json' \
  -H 'Authorization: Bearer <Secret key goes here>' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Content-Type: application/json' \
  -H 'Host: portal.klix.app' \
  -H 'accept-encoding: gzip, deflate' \
  -H 'cache-control: no-cache' \
  -d '{
   "recurring_token": "<Recurring payment token>"
}'
```
