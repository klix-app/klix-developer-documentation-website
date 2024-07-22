# Klix API

Klix provides unified API for multiple payment methods - card and PSD2 bank payments. Full up-to-date REST API Reference can be found [here](https://portal.klix.app/api). OpenAPI document that can be used to generate API client can be found [here](https://portal.klix.app/api/schema/v1/).
In order to call Klix API you should first receive API credentials (Brand ID and Secret Key) from Klix representative.

## API usage in single payment scenario

### Single payment step-by-step guide

1. Get list of payment methods (optional). This API end-point returns name and logo of each payment method available to merchant (Klix Card payments, Citadele and other bank PSD2 payments etc.). Use this list to render a payment methods page on your site.
2. Create a purchase by submitting order data to Klix. Once purchase is created link to Klix hosted payment page will be returned as `checkout_url` field value. After customer is redirected to this page Klix will handle the payment process.
3. Handle one of possible payment process outcomes:
    * If customer payment is successful then callback is sent to merchant server and customer is redirected to successful purchase page specified by merchant. Note that you should consider purchase as successfully paid only after callback is received and purchase status is checked. Note that in case if callback is not used you can call Klix API to get purchase status once customer is redirected back to successful purchase page.
    * If customer payment fails for some reason customer is redirected to failed purchase page.
    * If cancelled payment redirect URL is specified in purchase creation request then customer will be able to go back to merchant page from Klix payment page. It's preferable to specify checkout/payment method selection page as cancelled payment redirect URL so that customer can make adjustments in shopping cart or choose different payment method.

### Single payment request examples

These are simple request examples that illustrate Klix API usage. Always use [API Reference](https://portal.klix.app/api) as a single source of truth.
Note that `<Brand ID goes here>` and `<Secret key goes here>` should be replaced with actual `Brand ID` and `Secret Key` received from Klix contact person.

#### Get list of available payment methods

This operation allows you to retrieve all payment methods available to you at the current time. Note that payment method availability depends upon your agreement with Klix and it can also vary from time to time e.g. some bank payment method can be temporarily unavailable because of planned maintenance down-time. In such a case this bank payment method will be excluded from the available payment method list returned by this API end-point until planned maintenance is over and this payment method is re-enabled. That's one of the examples of why an available payment method list should be retrieved on a regular basis in order to offer only fully functional payment methods to the end-user.

!!! Warning "Do not call this API end-point before each purchase initiation since this API end-point is rate limited. Cache available payment methods on server side and call this API end-point not more often than once in a minute."

All payment methods currently supported by Klix are listed [here](/../payment-methods).

Note that this API end-point returns both payment method general availability, availability in country and additional information like human-readable payment method name and logo URI. 

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
   "purchase": {
      "language": "lv",
      "products": [
         {
            "price": 3000,
            "name": "Xiaomi Mi Smart Band 5"
         },
         {
            "price": 100,
            "name": "Screen protector for Xiaomi Mi Smart Band 5"
         }
      ]
   },
   "client": {
      "email": "test@test.com"
   },
   "brand_id": "<Brand ID goes here>",
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
   "purchase": {
      "language": "lv",
      "products": [
         {
            "price": 3000,
            "name": "Xiaomi Mi Smart Band 5"
         },
         {
            "price": 100,
            "name": "Screen protector for Xiaomi Mi Smart Band 5"
         }
      ]
   },
   "client": {
      "email": "test@test.com"
   },
   "payment_method_whitelist": ["klix"],
   "brand_id": "<Brand ID goes here>",
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

### Recurring payment step-by-step guide

1. Create an initial purchase with recurring flag set to `true` (see `"force_recurring": true` in [example](#create-an-initial-purchase-to-register-card-for-subsequent-recurring-payments)) in order to register payment card details for subsequent use in recurring payments. Created purchase `id` and `checkout_url` will be returned in a response. Redirect customer to `checkout_url` and Klix will handle the payment process. Store this initial purchase `id` on your side as a recurring payment token and use it later on to charge a customer payment card (see `<Recurring payment token>` placeholder in [example](#charge-customer-card-as-a-recurring-payment)). If purchase created in previous step is paid by customer then card details are successfully captured for subsequent use.
2. Once customer card should be charged in a form of a recurring payment new purchase should be created via API by specifying amount that needs to be charged and transaction description. This time `force_recurring` should be `false` or can be omitted at all (see [example](#create-recurring-payment)). In this case only purchase `id` returned in response should be used in next step (see `<Purchase id of recurring payment>` placeholder in [example](#charge-customer-card-as-a-recurring-payment)). Note that `checkout_url` should not be used.
3. Created purchase should be charged via API call. Note that both newly created purchase `id` created in step 2 and recurring payment token (purchase `id`) created in step 1 should be passed to this API end-point (see [example](#charge-customer-card-as-a-recurring-payment)).

### Recurring payment request examples

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
   "purchase": {
      "language": "lv",
      "products": [
         {
            "price": 700,
            "name": "XYZ service subscription"
         }
      ]
   },
   "client": {
      "email": "test@test.com"
   },
   "brand_id": "<Brand ID goes here>",
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
   "purchase": {
      "language": "lv",
      "products": [
         {
            "price": 700,
            "name": "XYZ service subscription fee for December 2020"
         }
      ]
   },
   "client": {
      "email": "test@test.com"
   },
   "brand_id": "<Brand ID goes here>",
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

## API usage in reservation scenario (payment execution separated from authorization)

### Reservation step-by-step guide

1. Create a purchase by submitting order data and additional `skip_capture` flag to Klix indicating card payment authorization separation from payment execution. Once purchase is created link to Klix hosted payment page will be returned as `checkout_url` field value. Payment identifier returned in purchase creation response should be stored for later use in capture or release requests. After customer is redirected to this page Klix will capture customer card details and will reserve the funds equal to purchase total amount.
2. There are two options how to proceed with reservation:
    * Capture reserved amount. There's an option to capture either full reserved amount or amount that's smaller than initially reserved amount.
    * Release (return to customer) reserved amount.

!!! Warning "Automatic release of reserved amount"
    Reservations are released automatically after 6 days if capture request is not received within this period.

### Reservation functionality request examples

These are simple request examples that illustrate Klix API usage. Always use [API Reference](https://portal.klix.app/api) as a single source of truth.
Note that `<Brand ID goes here>` and `<Secret key goes here>` should be replaced with actual `Brand ID` and `Secret Key` received from Klix contact person.

#### Create a reservation purchase

This option allows to create a purchase that will reserve purchase total amount in case of card payment. Store purchase `id` returned in response to either capture or release funds reserved by this purchase. Redirect customer to `checkout_url` returned in purchase creation response to allow customer to enter card details. Note that in case of successful reservation purchase status will be changed to "hold" instead of "paid" as in case of regular card payment.

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
   "purchase": {
      "language": "lv",
      "products": [
         {
            "price": 3000,
            "name": "Xiaomi Mi Smart Band 5"
         },
         {
            "price": 100,
            "name": "Screen protector for Xiaomi Mi Smart Band 5"
         }
      ]
   },
   "skip_capture": true,
   "client": {
      "email": "test@test.com"
   },
   "payment_method_whitelist": ["klix"],
   "brand_id": "<Brand ID goes here>",
   "reference": "Your order id"
}'
```

#### Capture funds reserved by previously created purchase

To capture full amount previously reserved by specified purchase no request body should be sent. `<Purchase ID>` is purchase identifier (field `id` value) received in purchase creation response.

```sh
curl --location --request POST 'https://portal.klix.app/api/v1/purchases/<Purchase ID>/capture/' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <Secret key goes here>'
```

#### Capture partial funds reserved by previously created purchase

To capture amount that's smaller than previously reserved amount this new amount should be specified in request. `<Purchase ID>` is purchase identifier (field `id` value) received in purchase creation response.

```sh
curl --location --request POST 'https://portal.klix.app/api/v1/purchases/<Purchase ID>/capture/' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <Secret key goes here>' \
--data-raw '{
    "amount": 5
}
'
```

#### Release funds reserved by previously created purchase

To release funds reserved by Purchase following request should be sent:

```sh
curl --location --request POST 'https://portal.klix.app/api/v1/purchases/<Purchase ID>/release/' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <Secret key goes here>'
```

## API usage in recurring payment & reservation scenario

Recurring payment & reservation scenario combines recurring payment and reservation scenarios - after customer performs initial card registration for recurring payments merchant can use this card for reserving initial amount and then either charging exact amount that's not greater than initially reserved amount or releasing reservation.

### Recurring payment & reservation step-by-step guide

1. Create an initial purchase with recurring flag set to `true` (see `"force_recurring": true` in [example](#create-an-initial-purchase-to-register-card-for-subsequent-recurring-payments)) in order to register payment card details for subsequent use in recurring payments. Created purchase `id` and `checkout_url` will be returned in a response. Redirect customer to `checkout_url` and Klix will handle the payment process. Store this initial purchase `id` on your side as a recurring payment token and use it later on to reserve initial amount (see `<Recurring payment token>` placeholder in [example](#reserve-recurring-payment-initial-amount)). If purchase created in this step is paid by customer then card details are successfully captured for subsequent use.
2. Create a new purchase once recurring purchase amount should be reserved. Additional `skip_capture` flag (see [example request](#create-recurring-payment-for-reservation)) should be added in request indicating card payment authorization separation from payment execution. Once purchase is created payment identifier returned in response should be stored for later use in a charge request. In this case only purchase `id` returned in response should be used in next step (see `<Purchase id of recurring payment>` placeholder in [example](#reserve-recurring-payment-initial-amount)). Note that `checkout_url` should not be used.
3. Make charge API call to perform actual reservation of recurring payment initial amount. Note that both newly created purchase `id` created in step 2 and recurring payment token (purchase `id`) created in step 1 should be passed to this API end-point (see [example](#reserve-recurring-payment-initial-amount)).
4. There are two options how to proceed with recurring payment reservation:
    * Capture reserved amount. There's an option to capture either full reserved amount or amount that's smaller than initially reserved amount.
    * Release (return to customer) reserved amount.

### Recurring payment & reservation request examples

These are simple request examples that illustrate Klix API usage. Always use [API Reference](https://portal.klix.app/api) as a single source of truth.
Note that `<Brand ID goes here>` and `<Secret key goes here>` should be replaced with actual `Brand ID` and `Secret Key` received from Klix contact person.

#### [Create an initial purchase to register card for subsequent recurring payments](#create-an-initial-purchase-to-register-card-for-subsequent-recurring-payments)

Create an initial purchase for card registration in a same way it's done in a regular recurring payment scenario.

#### Create recurring payment for reservation

This should be done for each recurring transaction (e.g. to reserve some predefined amount before electric car charge is started). 

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
   "purchase": {
      "language": "lv",
      "products": [
         {
            "price": 5000,
            "name": "Electric car charge"
         }
      ]
   },
   "client": {
      "email": "test@test.com"
   },
   "skip_capture": "true",
   "brand_id": "<Brand ID goes here>",
   "reference": "Your order id"
}'
```

#### Reserve recurring payment initial amount

Note that in case of successful reservation recurring purchase status will be changed to "hold" instead of "paid" as in case of regular card payment.

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

#### [Capture funds reserved by previously created purchase](#capture-funds-reserved-by-previously-created-purchase)

Note that after successful capture recurring purchase status is changed to "paid".

#### [Capture partial funds reserved by previously created purchase](#capture-partial-funds-reserved-by-previously-created-purchase)

Note that after successful capture recurring purchase status is changed to "paid".

#### [Release funds reserved by previously created purchase](#release-funds-reserved-by-previously-created-purchase)

Note that after successful capture recurring purchase status is changed to "released".

## API usage in bulk payment scenario

A bulk payment is a group of payments to multiple bank accounts (IBANs) which requires only single confirmation from customer.

Typical use case for bulk payment functionality is a product marketplaces where during a checkout customer pays to multiple merchants at once. 
Bulk payment functionality allows to initiate and confirm these multiple payments as a single payment.

Here is a list of the payment methods that currently support bulk payment functionality:

* seb_ee_pis
* seb_lt_pis
* seb_lv_pis
* swedbank_ee_pis
* swedbank_lt_pis
* swedbank_lv_pis

### Bulk payment step-by-step guide

1. Sign the agreement with Klix for the usage of bulk payment functionality. Submit the list of creditor IBANs for whitelisting.
2. Initiate a bulk payment by providing multiple creditors and amounts.
3. Handle a single successful payment callback for each bulk payment.

### Bulk payment request examples

These are simple request examples that illustrate Klix API usage. Always use [API Reference](https://portal.klix.app/api) as a single source of truth.
Note that `<Brand ID goes here>` and `<Secret key goes here>` should be replaced with actual `Brand ID` and `Secret Key` received from Klix contact person.

#### Create a bulk payment purchase

Initiate a purchase by specifying multiple creditors and corresponding payment amounts and descriptions. In the following example Swedbank Latvia bulk payment total amount is 0.03 EUR, and it consists of two payments:

* 0.02 EUR payment to John Doe's IBAN LVXXPARX0000000000001 with a payment description "Description of payment to John Doe"
* 0.01 EUR payment to Jane Doe's IBAN LVXXHABA0000000000001 with a payment description "Description of payment to Jane Doe"

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
    "purchase": {
        "products": [
            {
                "name": "Description of payment to John Doe",
                "price": 2
            },
            {
                "name": "Description of payment to Jane Doe",
                "price": 1
            }
        ],
        "payment_method_details": {
            "pis_bulk_purchase": [
                {
                    "creditor_name": "John Doe",
                    "creditor_iban": "LVXXPARX0000000000001"
                },
                {
                    "creditor_name": "Jane Doe",
                    "creditor_iban": "LVXXHABA0000000000001"
                }
            ]
        }
    },
    "client": {
        "email": "test@test.com"
    },
    "payment_method_whitelist": ["swedbank_lv_pis"],
    "brand_id": "<Brand ID goes here>`",
    "reference": "Your order id"
}'
```

After redirecting customer to Klix payment page (field `checkout_url` from init payment response) bulk payment details will be presented.

![Bulk payment example](../images/api/bulk_payment_example.png)

Note that all creditor IBANs should be whitelisted before bulk payment is initiated. In case at least one of the creditor IBANs is not whitelisted following error message is returned in a purchase initiation response:

```json
{
    "__all__": [
        {
            "message": "Creditor's IBAN 'LVXXPARX0000000000001' is not whitelisted. Please ensure the creditor's IBAN is added to the whitelist and try again.",
            "code": "invalid"
        }
    ]
}
```

