# Callbacks

## Callback URLs

During order lifecycle KLIX consumes API end-points implemented by merchant store. All URLs should be implemented and accessible via HTTPS in order to be callable by Klix.

| URL Type                      | Example                            | Description
|-------------------------------|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Terms & Conditions URL        | https://merchant.com/terms.html    | HTTP GET request is sent from user device (browser or mobile) to check merchant legally binding agreement text.    |
| Merchant Callback URL         | https://merchant.com/callbacks     | HTTP OPTIONS request is sent from Klix backend to check if HTTP connection can be established. This end-point should return HTTP status 200.|
| Order verification URL        | https://merchant.com/orders        | HTTP POST request is sent from Klix backend to this end-point whenever a customer submits an order in the checkout form. Merchant store should validate order data (check if selected product/service price is valid and is available in stock etc.) and either approve or reject an order. Note that Klix does not send full order information in notification request body. Only order identifier is sent as a JWS payload. You should check JWS signature, Base64 decode request payload to extract order identifier and use Klix API to retrieve order data by order identifier. Example JWS sent as a notification body: `eyJraWQiOiJNUGF5IiwiYWxnIjoiUlMyNTYifQ.eyJvcmRlcklkIjoiMWE2YTUzNjgtZTc5OS00OTY3LWI3NDItNjdhZGMxNmFlYzhhIn0.OZQU_2nPKUWl93U8YJJ9GjzZlLmmKS7ffuVu1WSZ5Y4JSU65LJxYs3kj0a460abXsBLkkVGX1Hi89fxNJ8CMsQ`. Base64 decoding middle part of this JWS token will lead to following JSON document `{"orderId":"1a6a5368-e799-4967-b742-67adc16aec8a"}`.
| Purchase notification URL     | https://merchant.com/purchases     | HTTP POST request is sent from Klix backend to confirm that payment has either been collected successfully on behalf of the merchant or either payment has been cancelled. JWS payload contains both order identifier and status e.g. `{"status":"PAYMENT_APPROVED","orderId":"dfe50808-1771-4015-98bd-dac1f16fa6c3"}` in case of successful payment or `{"status":"PAYMENT_REJECTED","orderId":"dfe50808-1771-4015-98bd-dac1f16fa6c3"}` in case of failed or cancelled payment. |
| Shipping cost calculation URL | https://merchant.com/shipping-cost | HTTP POST request is sent from Klix backend in order to calculate shipping costs for certain order. Note that this end-point should be implemented only in case merchant supports dynamic pricing delivery i.e. delivery price can be determined only after KLIX client has entered delivery address. Otherwise different delivery option cost can be passed as Klix [Widget](../widget/) parameter. See [Shipping cost calculation request JWS payload example](#shipping-cost-calculation-request-jws-payload-example)|

### Shipping cost calculation request JWS payload example

Note that `pickup_point` is present only if customer has selected a delivery to pickup point, `address` is present both in case of delivery to address specified by customer and delivery to pickup point.

``` javascript
{  
    "order_id": "05957e7f-803f-46f0-9100-1c3e10199b43",
    "order_items": [
        {
            "reference": "QZT-213",
            "quantity": 2.000
        },
        {
            "reference": "TP-LNK-3840",
            "quantity": 1.000
        }
    ],
    "shipping_method_id": "omniva",
    "address": {
        "city": "Rīga",
        "address": "K.Valdemāra 62, Rīga",
        "postal_code": "LV-1013"
    },
    "pickup_point": {
        "external_id": "9111",
        "name": "Rīgas Briāna ielas Rimi pakomāts",
        "comments": "Pa labi no galvenās ieejas",
        "service_hours": "P-Pk.piegāde 10:00, izņemšana 17:00 Sestdienās piegāde 14:00,izņemšana 14:00"
    }
}
```

## Updating callback URLs

URLs can be set in Merchant Console settings.

![alt_text](images/callback_urls.png "Callback URLs configuration in Merchant Console")
