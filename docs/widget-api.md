# Legacy Klix API (deprecated)

## Quick Start guide

 This 5 minute guide covers overview of Klix integration technical aspects.

### Integration steps

A central part of Klix checkout/payment solution is Klix web widget - HTML web component i.e. form for entering customer's credit card data. Klix widget accepts merchant order parameters (order identifier, amount, payment description, etc.) via HTML element attributes. The following steps describe actions that you need to perform in order to add Klix widget to your webshop and accept a callback on a server-side once payment has been successfully processed by Klix.

#### 1. Embed Klix widget into your web shop

In order to embed Klix widget into your web shop following HTML fragment referencing Klix widget JavaScript should be added to your web shop checkout page.

```html
<head>
    <script type="module"  
        src="https://klix.blob.core.windows.net/stage/widget/build/klixwidget.esm.js">
    </script>
    <script nomodule  
        src="https://klix.blob.core.windows.net/stage/widget/build/klixwidget.js">
    </script>
</head>
```

Note that Klix widget JavaScript code should be loaded from different destinations on production and test environments. See Testing integration [Environments](#environments) section for details.

#### 2. Pass order information to Klix widget

Next step is to place Klix widget HTML code in an appropriate place on a checkout page where this widget will be rendered and pass order/payment related information.

```html
<klix-checkout widget-id="21ca7904-ff16-48b5-918d-c2d80af81f05"  
    amount="5.45"  
    currency="EUR"  
    label="Order No 12345678"  
    language="lv"
    back-to-merchant-url="https://developers.klix.app/"
    signature="E2Ag7p/v9cUQ1U5ntkYIZuDwXPErBxZWA19mGFaW/hRDXlnWvY8UH5jF991/fTgWE5Ls/3DHi5cbbZbM7zkGBT/MB13oVt6QJEtJLIFHi8t5Oqd61S6nSYcKgB/Ju4FvqbQcSaQavc1VS9zd8sgq4BuUzhyF4rb/ErI5wq6VqvDfJLNneL3kfxAcoxtU1lVvYoqZWFz52MN57aFwa7A9wJJRGmpETXFl6jYeFLLYTUjb67eiuMs60vxGh8CFpNCXlJpHzVV3cMTb+h7xit+HCwUDvM8mXwlPWXzg5F4orsoHPHWbMx2HXh8BuwZ5uXnwJar0zFioYlKqLZ+qxje0Bg==">
</klix-checkout>
```

Note that attribute `signature` should contain valid order signature. See section [Signing order](#signing-order) for detailed instructions on how to generate a valid signature. Attribute `back-to-merchant-url` allows to specify a URL where customer will be redirected in case of cancelled purchase.
Here's Klix widget that corresponds to previously mentioned HTML code.

<!-- markdownlint-disable MD033 -->
<details>
    <summary>Click to load Klix widget</summary>

<div>
    <klix-checkout widget-id="21ca7904-ff16-48b5-918d-c2d80af81f05" amount="5.45" currency="EUR" label="Order No 12345678" language="lv" signature="B+nre6Oe6lnjh0hcW5dhOtRmXxN3pm6Sup3kjcNeQiSmTN6zQCp6kHErX/s+JIvkLIqQxD2D/EU2MUraQC03RyKHyX/Wr8qVVbPeBaskPkYR7l397BBYOghvVN1LS8RWdpQ4Q67kMYdPutqnJAUGJtHA51i14xmnaIRxctpK4UJE3qtfu1QjWPez/yP1lT/igpCTL66lqXKcbHac75v++5WUwwT5fCEUklPxudzC3qbujNhXZBPwAZxa2GaYQDzCOP7p/bcJgH/DwsaVMiDtekG5ANgXB51WOPB9X3pP1rdr6kbVccXhN0D4UrxMt3ZA4bPw+LaAWzVRNaVOJoNpZg=="></klix-checkout>
</div>
</details>
<!-- markdownlint-disable MD033 -->

#### 3. Implement an end-point that will be invoked upon payment completetion

After payment is completed (both successfully or failed) Klix server will send a callback HTTP request to your API end-point defined in Merchant Console. See [Purchase notification HTTP request example](#purchase-notification-request-example).
Note that first thing upon receiving purchase completed HTTP request you should verify request signature in order to ensure that request was sent by Klix server. See [Callback payload signature validation](#callback-payload-signature-validation) for details.

## Step by step integration instructions

### 1. Get access to Klix Merchant Console

Klix Merchant Console is a merchant self-service web-page for merchant profile and order data management.
In order to receive access to Merchant Console contact [support@klix.app](mailto:support@klix.app?subject=Merchant%20onboarding%20request&body=Hello.%0D%0AWe would like to request access to Klix checkout solution.%0D%0AKlix Merchant Console and API will be accessed from the following IP addresses on the test environment: PLEASE_REPLACE_THIS_WITH_IP_ADDESS%0D%0AE-mail address to access Merchant Console: PLEASE_REPLACE_THIS_WITH_EMAIL_ADDRESS) and request merchant onboarding in Klix.
Make sure to specify IP address from which Klix solution will be accessed and e-mail address for logging into Merchant Console. Merchant Console invitation e-mail will be sent after the request has been reviewed.

### 2. Decide which integration scenario suits you best

There are three types of integration scenarios currently supported by Klix. Choose one depending on a type of your existing webshop:

1. Simple static merchant website integration. For each product separate Klix Instant Widget should be created and widget's HTML code should be placed on merchant's product page. In such case only specific product(s) can be purchased using specific widget and for each product(s) a separate widget should be created. Orders and payments are fully managed in Klix Merchant Console. Therefore this integration type is suitable for rather small product catalog (up to 10 - 15 products).
![Simple integration using instant widget](images/instant_widget.png "Instant widget")

2. Klix Pay. Klix widget is integrated into custom built or standard e-commerce platform webshop and is used just as a payment option. In such case webshop's existing checkout page is responsible for collecting customer's information including delivery addressw. It means that Klix  Single Klix Checkout Widget can be created to handle multi-product shopping carts and unlimited product catalogs. This requires [callbacks](#callbacks) implementation in merchant's web store.

    <!-- markdownlint-disable MD033 -->
    <details>
    <summary>Klix Pay example</summary>

    <klix-checkout widget-id="21ca7904-ff16-48b5-918d-c2d80af81f05" amount="5.45" currency="EUR" label="Order No 12345678" language="lv" signature="B+nre6Oe6lnjh0hcW5dhOtRmXxN3pm6Sup3kjcNeQiSmTN6zQCp6kHErX/s+JIvkLIqQxD2D/EU2MUraQC03RyKHyX/Wr8qVVbPeBaskPkYR7l397BBYOghvVN1LS8RWdpQ4Q67kMYdPutqnJAUGJtHA51i14xmnaIRxctpK4UJE3qtfu1QjWPez/yP1lT/igpCTL66lqXKcbHac75v++5WUwwT5fCEUklPxudzC3qbujNhXZBPwAZxa2GaYQDzCOP7p/bcJgH/DwsaVMiDtekG5ANgXB51WOPB9X3pP1rdr6kbVccXhN0D4UrxMt3ZA4bPw+LaAWzVRNaVOJoNpZg=="></klix-checkout>

    </details>
    <!-- markdownlint-disable MD033 -->

3. Klix Checkout. Just as in Klix Pay scenario Klix widget is integrated into custom built or standard e-commerce platform webshop but in this case Klix is used as a full checkout solution. It means that order information is still maintained in merchant's webshop and during checkout process order information is passed to Klix wiget which is responsible for collecting customer's information including delivery address. Same as in Klix Pay scenario both after successful and failed payment merchant's [callback](#callbacks) is invoked by passing both order, customer and delivery information.

    <!-- markdownlint-disable MD033 -->
    <details>
    <summary>Klix Checkout example</summary>

    <klix-checkout widget-id="8c597447-5234-4de1-ab85-b54e252098ce" language="lv" order="{&quot;items&quot;: [{&quot;orderItemId&quot;: &quot;12345&quot;, &quot;amount&quot;: 88, &quot;taxRate&quot;: 0.21, &quot;currency&quot;: &quot;EUR&quot;, &quot;label&quot;: &quot;Vacuum cleaner TP-3&quot;}, {&quot;amount&quot;: 9.39, &quot;currency&quot;: &quot;EUR&quot;, &quot;label&quot;: &quot;TP-3 HEPA filter&quot;,&quot;count&quot;: 2, &quot;unit&quot;: &quot;PIECE&quot;}], &quot;shippingOptions&quot;: [{&quot;id&quot;:&quot;omniva&quot;,&quot;amount&quot;:2,&quot;currency&quot;:&quot;EUR&quot;,&quot;taxRate&quot;: 0.21},  {&quot;id&quot;:&quot;courier&quot;,&quot;amount&quot;:0}]}" signature="vy1KbYGxD2oZtSGvkoVLt8ZdHCkecbFpK9L7lsyK01D6FNvzTg3L3XF894sX68mmmPBm3BjMdw8QPsbz4d68aJu6WKj9hk5qu2sPmXKyt7ZgIVdHZJwI729g+Z5MFHDfmqbG1JBlQSYpMRrA1NX8zi1d4/+Sono1huvuc0H422BrxUt//CJSfug91mp/ZSkb7q5KiPZaZBpPHyNtNSuwvHUc5WO5KX2En0j4+8q/O3ekZEEDa5dsdhG4G2nZdb3ti8Wa4DQ+NTYmQ170QVoWtQwlleJ7MbP03K2kxikUqhEShLGfKkoFuhtkih0654ZnsG7AYs7ga8WvXEZ7SB2DHQ=="></klix-checkout>

</details>
<!-- markdownlint-disable MD033 -->

### 3. Provide integration configuration in Merchant Console

Follow registration link in Merchant Console invitation e-mail and specify new password. After registration follow [these steps](#integration-configuration) to configure integration.

### 4. Add widget HTML code to your web-page

Add Klix widget HTML code to your product or checkout page depending on a type of used widget (instant, checkout, payment) and your webshop specifics. More information about widget configuration can be found in [Widget](#klix-widget) section.

### 5. Implement purchase completed callback end-point

[Callback](#callbacks) end-point should be implemented so that Klix can send payment status to merchant.

### 6. Implement callbacks to navigate back to payment method selection page or display additional message in case of successfull payment

JavaScript [callaback](#navigate-back-to-payment-method-selection-shopping-cart-page) should be implemented in order to allow customer to navigate back to payment method selection page or checkout page in case of failed or cancelled Klix payment.
Additional [callback](#payment-completed-callback) can be implemented in order to adjust payment status page depending on a payment result.

### 7. Test integration

Klix provides fully functional test environment that can be used to perform end to end [integration implementation testing](#integration-testing).

### 8. Sign agreement and request access to production environment

After integration has been successfully tested on test environment you can proceed with creadentials request for production.

### 9. Pass checklist on production environment

After integration configuration is completed on production environment you need to make test payments and pass few other tests from production readiness checklist.

## Integration configuration

Integration configuration is done in Klix Merchant Console. After logging in Merchant Console follow these steps to configure Klix integration.

### 1. Complete the merchant profile in Merchant Console

Fill in merchant data in "Business Profile view". Specify URL to merchant's terms & conditions. Specify [callback URLs](#callbacks) in case advanced integration (API & callbacks based) is used.

![Fill in merchant data in Business Profile view](images/merchant_console_business_profile.png "Merchant business profile")

### 2. Download Klix public key

Head to Certificates page and copy public key contents from "Service Provider Certificate info" section.

![Copy Klix public key](images/merchant_console_certificates.png "Klix public key")

Store public key contents in file and use this key to [validate](#callback-payload-signature-validation) the Klix callback payload signature.

!!! Warning "Callback payload signature validation"
    Merchant is obligated to perform Klix callback payload signature validation using Klix public key in order to ensure request payload integrity and authenticity.

### 3. Generate private/public key pair for signing the API requests

Generate private/public key pair and download private key file. Merchant order details passed as Klix widget attributes and all data modification requests sent to Klix API e.g. payment refund should be signed using this private key. Klix will use merchant certificate public key to check each data modification request payload integrity and authenticity. In order to understand which merchant's certificate should be used to validate a HTTP request payload HTTP header `X-Klix-Key-ID` value should be specified. Header value can be obtained from Certificates page corresponding certificate's field "Name".

!!! Warning "Security notice"
    Merchant is reponsible for storing certificate's private key securely. Merchant can generate a new certificate in Merchant Console and use this certificate to sign request payload and widget attrributes. Note that in such case HTTP header `X-Klix-Key-ID` value and Klix widget attribute `certificate-name` should match new certificate's name.

![Create new merchant certificate](images/merchant_console_certificate_created.png "New merchant certificate creation")

### 4. Create a widget

Head to Widgets section to create a new widget. Klix widget is Klix form configuration representation that is identifiable by it's id. There are tree types of widgets:

* Static widget
* Klix Pay widget
* Klix Checkout widget

See [Widget](#klix-widget) section for more detailed description of widget types and configuration.

### 5. Store API key

Note that Klix standard checkout flow doesn't require a merchant to invoke Klix API. More advanced use cases like refunding an order can be handled both in Merchant Console or via Klix API. For each HTTP request sent to Klix API HTTP header called `X-KLIX-Api-Key` should be specified. Header value can be obtained from Certificates page "Merchant Api key info" section's field "Api key".

![Add Klix API key to HTTP request headers](images/merchant_console_api_key.png "Klix API key")

## Callbacks

### Callback URLs

During order lifecycle Klix invokes API end-points implemented by merchant store. All URLs should be implemented and accessible via HTTPS in order to be callable by Klix.

!!! Warning "Exposing you API via HTTPS on development environment"
    If you have trouble to expose your API via HTTPS on a development environment you can use [ngrok](https://ngrok.com).

| URL Type                      | Example                            | Optional | Description
|-------------------------------|------------------------------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Purchase notification URL     | https://merchant.com/purchases     | false    | HTTP PUT request is sent from Klix backend to confirm that a payment has either been collected successfully on behalf of the merchant or either payment has failed/was cancelled. Note that error code and message might not be present even in case of failed payment. See [Purchase notification request example](#purchase-notification-request-example).|
| Terms & Conditions URL        | https://merchant.com/terms.html    | false    | HTTP GET request is sent from user device (browser or mobile) to retrieve merchant legally binding agreement text.    |
| Merchant Callback URL *        | https://merchant.com/callbacks    | true     | Connection test end-point. HTTP OPTIONS request is sent from Klix backend to check if HTTP connection to merchant server can be established. This end-point should return HTTP status 200 and header Access-Control-Allow-Methods should contain POST method.|
| Order verification URL        | https://merchant.com/orders        | true     | Deprecated in favour of [order signing](#signing-order). Used only in case of Klix widgets with valiation type `CALLBACK`. HTTP POST request is sent from Klix backend to this end-point whenever a customer submits an order in the checkout form. Merchant store should validate order data (check if selected product/service price is valid and is available in stock etc.) and either approve or reject an order. Note that Klix does not send full order information in notification request body. Only order identifier is sent as a JWS payload. You should check JWS signature, Base64 decode request payload to extract order identifier and use Klix API to retrieve order data by order identifier. Example JWS sent as a notification body: `eyJraWQiOiJNUGF5IiwiYWxnIjoiUlMyNTYifQ.eyJvcmRlcklkIjoiMWE2YTUzNjgtZTc5OS00OTY3LWI3NDItNjdhZGMxNmFlYzhhIn0.OZQU_2nPKUWl93U8YJJ9GjzZlLmmKS7ffuVu1WSZ5Y4JSU65LJxYs3kj0a460abXsBLkkVGX1Hi89fxNJ8CMsQ`. Base64 decoding middle part of this JWS token will lead to following JSON document `{"orderId":"1a6a5368-e799-4967-b742-67adc16aec8a","externalOrderId":"1233456789"}`, where `orderId` is Klix order identifier and `externalOrderId` is order identifier in merchant's system.|
| Shipping cost calculation URL | https://merchant.com/shipping-cost | true     | HTTP POST request is sent from Klix backend in order to calculate shipping costs for certain order. Note that this end-point should be implemented only in case merchant supports dynamic pricing delivery i.e. delivery price can be determined only after Klix client has entered delivery address. Otherwise different delivery option cost can be passed as Klix [Widget](#klix-widget) parameter. See [Shipping cost calculation request example](#shipping-cost-calculation-request-example).|

#### Purchase notification request example

HTTP status code 200 should be returned by this API end-point otherwise Klix server will retry to send notification multiple times. In order to ensure that a purchase notification was sent by Klix and request was not altered it's important to [check a request signature](#callback-payload-signature-validation). In case of a successfull payment `"status"` field value is `"PAID"` and order in merchant web shop can be marked as paid.

```bash
#!/bin/bash
curl -x PUT https://your.site/payment-notifications \
    -H 'Content-Type: application/json' \
    -H 'X-Klix-Signature: MKqXr7siOkC6TYENeHUcy5ofFDiWpqMt+ow5iWJqnIYWU71W50fZFHfy3BVrehEGCvf+TufZK6DPymdM1e2G0w==' \
    -d '{
        "id":"d72096a0-58f2-46f0-9a4c-6d2271784530",
        "status":"PAID",
        "customer":{
            "first_name":"John",
            "last_name":"Doe",
            "email":"john.doe@klix.app",
            "phone_number":"37120000000"
        },
        "payment":{
            "accountStatementReference":"731589767",
            "method": "CARD",
            "status": "FAILED",
            "error": {
                "code": "129",
                "message": "Decline, suspected counterfeit card"
            }
        },
        "tax_amount":18.53,
        "total_amount":108.78,
        "items":[
            {
                "amount":72.73,
                "label":"Vacuum cleaner TP-3",
                "tax_amount":15.27,
                "total_amount":88.00,
                "tax_rate":0.21,
                "order_item_id":"12345",
                "quantity":1.000,
                "unit":"PIECE",
                "type":"UNKNOWN"
            },
            {
                "amount":6.13,
                "label":"TP-3 HEPA filter",
                "tax_amount":3.26,
                "total_amount":18.78,
                "tax_rate":0.21,
                "quantity":2.000,
                "unit":"PIECE",
                "type":"UNKNOWN"
            },
            {
                "amount":2.00,
                "label":"Piegāde",
                "tax_amount":0.00,
                "total_amount":2.00,
                "tax_rate":0.00,
                "quantity":1.000,
                "unit":"PIECE",
                "type":"SHIPPING"
            }
        ],
        "currency":"EUR",
        "merchant_urls":{
            "confirmation":"https://webhook.site/#!/6ddabff9-49af-4d4d-b221-7b607ffed276"
        },
        "shipping":{
            "type":"PICKUP_POINT",
            "address":{
                "country":"Latvia",
                "city":"Rīga",
                "street":"Āzenes iela 5",
                "latitude":24.08128,
                "longitude":56.949924,
                "postal_code":"9934"
            },
            "method":{
                "id":"omniva",
                "name":"Omniva"
            },
            "contact_phone":"37120000000",
            "pickup_point":{
                "name":"Rīgas T/C Olimpia pakomāts",
                "comments":"Blakus iebrauktuvei pazemes autostāvvietā",
                "external_id":"9934",
                "service_hours":"P-Pk.piegāde 10:00, izņemšana 17:00 Sestdienās piegāde 14:00,izņemšana 14:00"
            }
        },
        "effective_amount":108.78
    }'
```

#### Shipping cost calculation request example

Note that `pickup_point` is present only if customer has selected a delivery to pickup point, `address` is present both in case of delivery to address specified by customer and delivery to pickup point.

```bash
#!/bin/bash
curl -x POST https://your.site/shipping-cost-calculations \
    -H 'Content-Type: application/json' \
    -H 'X-Klix-Signature: TODO' \
    -d '{
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
            "service_hours": "P-Pk. piegāde 10:00, iznemšana 17:00 Sestdienas piegāde 14:00, iznemšana 14:00"
        }
    }'
```

### Callback payload signature validation

First thing upon receiving purchase completed HTTP request you should verify request signature in order to ensure that request was sent by Klix server. Signature is sent as HTTP header `X-Klix-Signature` value and should be verified using `SHA256WithRSA` algorithm and Klix public key that can be downloaded from Merchant Console. Example signature validation code:

```PHP tab=
<?php
function is_signature_valid($payload, $signature_header_value) : int {
    $klix_public_key = <<<EOD
-----BEGIN CERTIFICATE-----
MIIB5TCCAY+gAwIBAgIENkY2rzANBgkqhkiG9w0BAQsFADBoMQswCQYDVQQGEwJM
VjEQMA4GA1UECBMHVW5rbm93bjEQMA4GA1UEBxMHVW5rbm93bjERMA8GA1UEChMI
Q2l0YWRlbGUxEDAOBgNVBAsTB1Vua25vd24xEDAOBgNVBAMTB1Vua25vd24wHhcN
MTgwNjEyMTMzNjM4WhcNMjgwNjA5MTMzNjM4WjBoMQswCQYDVQQGEwJMVjEQMA4G
A1UECBMHVW5rbm93bjEQMA4GA1UEBxMHVW5rbm93bjERMA8GA1UEChMIQ2l0YWRl
bGUxEDAOBgNVBAsTB1Vua25vd24xEDAOBgNVBAMTB1Vua25vd24wXDANBgkqhkiG
9w0BAQEFAANLADBIAkEAqAUyLiFAd4hxAh3LrbBrbqk+lmGPVFgS3996vTCQ/L/h
L9WnA+EPnxMV5LFyd49xsf5bbspaLrXnVmwkuvUC9wIDAQABoyEwHzAdBgNVHQ4E
FgQUzbA4JwE+SOUOJEd25iwpd9cajJMwDQYJKoZIhvcNAQELBQADQQBDtypgN8O3
AZ+H4CjH5Ihq+V5i/a3pL6nj8Dg502wejDN8fXZJjJvdu0VxRzf4k41xeRg3lO7I
IrWkkFCW0LSH
-----END CERTIFICATE-----
EOD;

    return openssl_verify($payload, base64_decode($signature_header_value), $klix_public_key, OPENSSL_ALGO_SHA256);
}
?>
```

```PHP tab="Klix PHP library"
<?php
use Klix\Callback\ProviderSignatureValidator;
use Klix\Callback\ProviderCallbackRequestDecoder;

$validator = new ProviderSignatureValidator($apiConfiguration);
$validator->isValid(payload, signature_header_value);
?>
```

```Java tab=
import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.nio.charset.StandardCharsets;
import java.security.InvalidKeyException;
import java.security.NoSuchAlgorithmException;
import java.security.PublicKey;
import java.security.Signature;
import java.security.SignatureException;
import java.security.cert.CertificateException;
import java.security.cert.CertificateFactory;
import java.security.cert.X509Certificate;
import java.util.Base64;

private boolean isSignatureValid(String payload, String signatureHeaderValue) {
    byte[] decodedSignatureHeaderValue = Base64.getDecoder().decode(signatureHeaderValue);
    Signature signature = getSignature(payload);
    try {
        return signature.verify(decodedSignatureHeaderValue);
    } catch (SignatureException e) {
        throw new RuntimeException(e);
    }
}

private Signature getSignature(String payload) {
    PublicKey publicKey = loadPublicKey();
    try {
        Signature signature = Signature.getInstance("SHA256withRSA");
        signature.initVerify(publicKey);
        signature.update(payload.getBytes(StandardCharsets.UTF_8));
        return signature;
    } catch (NoSuchAlgorithmException | InvalidKeyException | SignatureException e) {
        throw new RuntimeException(e);
    }
}

private PublicKey loadPublicKey() {
    String klixPublicKey = "-----BEGIN CERTIFICATE-----\n" +
            "MIIB5TCCAY+gAwIBAgIENkY2rzANBgkqhkiG9w0BAQsFADBoMQswCQYDVQQGEwJM\n" +
            "VjEQMA4GA1UECBMHVW5rbm93bjEQMA4GA1UEBxMHVW5rbm93bjERMA8GA1UEChMI\n" +
            "Q2l0YWRlbGUxEDAOBgNVBAsTB1Vua25vd24xEDAOBgNVBAMTB1Vua25vd24wHhcN\n" +
            "MTgwNjEyMTMzNjM4WhcNMjgwNjA5MTMzNjM4WjBoMQswCQYDVQQGEwJMVjEQMA4G\n" +
            "A1UECBMHVW5rbm93bjEQMA4GA1UEBxMHVW5rbm93bjERMA8GA1UEChMIQ2l0YWRl\n" +
            "bGUxEDAOBgNVBAsTB1Vua25vd24xEDAOBgNVBAMTB1Vua25vd24wXDANBgkqhkiG\n" +
            "9w0BAQEFAANLADBIAkEAqAUyLiFAd4hxAh3LrbBrbqk+lmGPVFgS3996vTCQ/L/h\n" +
            "L9WnA+EPnxMV5LFyd49xsf5bbspaLrXnVmwkuvUC9wIDAQABoyEwHzAdBgNVHQ4E\n" +
            "FgQUzbA4JwE+SOUOJEd25iwpd9cajJMwDQYJKoZIhvcNAQELBQADQQBDtypgN8O3\n" +
            "AZ+H4CjH5Ihq+V5i/a3pL6nj8Dg502wejDN8fXZJjJvdu0VxRzf4k41xeRg3lO7I\n" +
            "IrWkkFCW0LSH\n" +
            "-----END CERTIFICATE-----";
    try (InputStream inputStream = new ByteArrayInputStream(klixPublicKey.getBytes())) {
        CertificateFactory fact = CertificateFactory.getInstance("X.509");
        X509Certificate certificate = (X509Certificate) fact.generateCertificate(inputStream);
        return certificate.getPublicKey();
    } catch (IOException | CertificateException e) {
        throw new RuntimeException(e);
    }
}
```

### Updating callback URLs

URLs can be set in Merchant Console settings.

![alt_text](images/callback_urls.png "Callback URLs configuration in Merchant Console")

## Security

Every data modification request sent by merchant to Klix and every Klix widget order parameters should be signed by merchant private key and validated on Klix side by merchant public key.

### Transport-Level Security

All communication is going via HTTPS TLS 1.2 protocol.

### Certificates

Private and public key pairs are issued by Klix PKI (Public Key Infrastructure) server. It is possible to generate a new RSA 2048 key pair in Merchant Console -> Certificates page. When clicking “GENERATE NEW CERTIFICATE” private key is downloaded to user machine, and public key is kept in Klix database.

![alt_text](images/merchant_console_generate_new_certificate.png "Generate merchant certificate in Merchant Console")

Merchant RSA Key Pair generation guidelines:

* Each key pair is assigned a unique identifier in Klix database.
* Merchant private key is not stored in Klix and its responsibility of merchant to keep it secure.
* Key could be revoked by the merchant or Klix admin at any time.
* Multiple keys could be active at the same time. Widget attribute `certificate-name` helps to identify which key is used to sign particular order.
* Default Key expiration time is 2 calendar years. This is defined on PKI policies level.
* Notifications about key status changes (as well as expiration reminders) is sent to merchant email address.

Requests sent from Klix to merchant defined callback URL are also signed digitally using Klix private key and validated using public key, which is available in Merchant Console.

!!! Warning "Storing private key"
    Do not share your private key with anyone.

### Signing order

In order for Klix server to be able to verify a payment request origin Klix widget order parameters must be signed by merchant. Note that if your webshop is written in PHP you can use [Klix PHP library](https://github.com/klix-app/merchant-api-php#generate-widget-html-representation) to sign orders therefore following section can be skipped.

#### Signature generation

Order signature is generated by concatenating order fields into single string and applying SHA256 hash function and RSA encryption to it.
Order signature = `Base64Encode(SHA256withRSA(signature-input))`, where `signature-input` is calculated in a following way.
![Generate signature](images/signature-generation.svg "Signature generation algorithm")
Note that all blank fields should be replaced with empty strings, all amount fields should be formatted with 2 decimals using dot as a decimal point and without thousand separator (e.g. if amount is 1025 then it should be formatted as 1025.00), count field should be formatted with 3 decimals (e.g. 2 should be formatted as 2.000).

#### Examples

Following private key is used in all examples. Note that you can obtain your merchant private key in Klix Merchant Console.

<!-- markdownlint-disable MD033 -->
<details>
    <summary>Expand to see a private key</summary>
  
    ```txt
    -----BEGIN PRIVATE KEY-----
    MIIEvAIBADANBgkqhkiG9w0BAQEFAASCBKYwggSiAgEAAoIBAQDAom+89QBxPCJz
    lQP+PtVsJrdl7wJq2Nj2crBDyNyZjIZcCaf7CCLRD43VCwsFjGSGoWhYDwbWAWzy
    ryi6bwcaXdWg8p/pHoWTP3sHluVaHQx6fh3xHsb2qgSE320JtvEYJC3k0RwQyt7j
    MEhN79Dizxj2fsMl9nv2fe1/8WwEudtaJl2o+iEQvhz9p2XR106AOU8+DesOt/1X
    9wGmQKpxffOLbq0WpYzXw7U5R41Zk9atusRi+CP5FAhKvdRQu9zv18vU2Mv2CdHl
    Dxe/Z+dpm3iFbBFKVPRMFHzBE4tYQ49YfHrxTob6rHbv1MoyGW01FgVhW989Caq2
    Sa60iV4XAgMBAAECggEAYG7w0P81gXpBJOtsXFLINKQNG5JS+3oi4vz3EHf1N0kJ
    2flbhmfERjQrJIOquNRXXq5etfaPzYJZMARPt0TtoV1oHHJZazJEXDpjNKFtn3b7
    UDbidlqZ79GdAYd35m0WtKcn8mxcSGvGuKTrDVkIkQJr1LoQp2iFFhhkVLBfCt3A
    W13MeCq4fcu5CHj7ZHExpgTkyO+o4cKCU+XqBMiN0xl2yt8BeFVpyuK0bxMs8yjb
    budT5mhA+pQYYLblS/S+ONYFhQZdvwyCUDcpmDwQc5oUCyJISPvT618cAL9/WfWU
    YhDjPZ1LDkiGcP6KWyEp4TCupMOCDQ6KoKdbfEdHAQKBgQD13sE1E5327ZnuqmYD
    4c+zD4c3duXi9ZqoD/ZYV9XwiER2XnMKMbDDztaPiBZVd5gy60I7uLXs/7elQ02D
    zla+vIew44ZgZUFuNnVCooT5MEGIrI6lyZEy7wOiMSSQsw60wbsDAqWqNZm63wbf
    fizIpvc3ELxXXDfqfgBpM1GJJwKBgQDIkjG4/E2dXhlUDxKozwi+5y/61Ir1jBKg
    c7KF5BbO61o/J7GxUcUDtf4Rw9wqaYWrcIycyIB0wGQKuWomadi1uM3m63x44cqQ
    RzcwUZckiQv465rpct/exXoq5xy8JdLMi9pVjGaAKtvDy1moVVMqf44704JTYBjK
    pqLRPkc5kQKBgFBh3SSvHyvFjlfa2bNXNgJCh2B4vLuwtDlRzqYfQVB8HHXedxnU
    GQhLLRaL1VFn4hhzlZnUz5WGHNIo6MS4xt9g37PTDvGg6P1RTJDWdUbKogI+9/v3
    pELUpYl3K48rGxm5ZJydVvAIyhOwKp0SsS2AeL5F+G1PNvCw9Q1vu65tAoGAGeNc
    zZ6rOfOqW0Iyy10lqNZYSAzk/rqcsN8eHNAK7ZxNIrex5MxEHK2B5LMeGM9T9IPr
    clUi1275urSh8eFZmmMHGJYuk7+5EJRQmEj/tDSIOhHXBejQ4oUktqLr/5D2O3aV
    sNMVa9ecVBJMOUECIgsixbAQM0jFCmslutJKCTECgYA+SCrepQmwfQF47I+wVHDf
    81Hvh3/WqeFaaXt12kvEk2DDdG9PYvEuqtEFalvH/5LcsWDiknA1zdo9pXLIg95N
    SY5hDphsXnZLaYS9V9uaW7RCC4VpWELxoVG13mtGDnteAIgoxwWIQ6TfhidFpVtM
    2KcJhf0hnLkFQW0qEMn0vg==
    -----END PRIVATE KEY-----
    ```
</details>
<!-- markdownlint-disable MD033 -->

##### Example No 1: Klix Pay widget with single order item

Consider Klix widget for following order should be placed in merchant webshop:

```html
<klix-checkout widget-id="21ca7904-ff16-48b5-918d-c2d80af81f05"  
    amount="5.45"  
    currency="EUR"  
    order-id="12345678"  
    label="Order No 12345678"  
    language="lv"
    back-to-merchant-url="https://example.com/shopping-cart"
    signature="n0b7Sj0qEVlU52kNctEHR9zUZ9pRtPjA/5/avPSQamx7HiI3q6qijgstBw6KhOKZqcCIL3RULbWNu6xoSGnNtW/nx+RcSd12I0st21Los9MXXPakEIjIto+2Zx0+ZiVoa97dxO8/iGF5A1U4qW9GFhPJGPqQecmZSp7rYaiO+VRq5D9KqKqRpBQEYN9YJgDgWMn36KVYkTdlOYAhJslwkVeKKZ+/ifUqHhiXbPKD3VKAXwx7/MqSiRlfU8Qsm7Vcv/zV05X9trZiaSYOL6yd9aWO/KE2so2hAswY58i6dR218/XD6ab5xTpCSXrfjYbfhInchukvlH7CrbE1T3RcWw==">
</klix-checkout>
```

Steps to calculate a signature:

1. Calculate signature input string. `Input string = CONCAT(widget ID, language, back to merchant URL, amount, currency, label) = 21ca7904-ff16-48b5-918d-c2d80af81f05lvhttps://example.com/shopping-cart123456785.45EUROrder No 12345678`. Note that fields without a value e.g. certificate name, success URL, unit, count are not included in signature input string.

2. Calculate signature using private key previously downloaded from Klix Merchant Console and applying SHA256withRSA and Base64Encode functions to calculated input string. `Signature = Base64Encode(SHA256withRSA(signature-input)) = Base64Encode(SHA256withRSA("21ca7904-ff16-48b5-918d-c2d80af81f05lvhttps://example.com/shopping-cart123456785.45EUROrder No 12345678"))`.

Calculated signature which should be passed to Klix widget as a `signature` attribute value in this example: `ebSnfsTOV2v0zwXxk8YVYjH6iYTfkYA+tn+ove9I+0Zi3lgQ+Mhc6sNJJlhqaSa6hqODZJKGGOCy/d2sqB86dIcJd2M3wB1MqHtVTNadCR+V3gHi+ORTcAQmX/c7crU0lLiKwfHSvaE7FlZUG9cU8ErcZfWDAJEtOMWncMl/yIeec1kPwb302mFzZUsXkVi8/VPp70k9oIh+vPvew/vVTGhoS2ClnFx4heezp1BNXYxZZsFoSgYFP555tNsHYi4GY6DOIx0qrUtmRrWCrt8HjWV7hBFPDNOQnQ05vALLcWtzWdDtGIo5a46HrlW2biCXW3R6K9jW/FxhsEVvwT7fDQ==`.

##### Example No 2: Klix Checkout widget with multiple order items and order delivery

Consider Klix widget for following order should be placed in merchant webshop:

```html
<klix-checkout widget-id="8c597447-5234-4de1-ab85-b54e252098ce" 
    language="lv"
    order="{&quot;items&quot;: [{&quot;orderItemId&quot;: &quot;12345&quot;, &quot;amount&quot;: 88, &quot;taxRate&quot;: 0.21, &quot;currency&quot;: &quot;EUR&quot;, &quot;label&quot;: &quot;Vacuum cleaner TP-3&quot;}, {&quot;amount&quot;: 9.39, &quot;currency&quot;: &quot;EUR&quot;, &quot;label&quot;: &quot;TP-3 HEPA filter&quot;,&quot;count&quot;: 2, &quot;unit&quot;: &quot;PIECE&quot;}], &quot;shippingOptions&quot;: [{&quot;id&quot;:&quot;omniva&quot;,&quot;amount&quot;:2,&quot;currency&quot;:&quot;EUR&quot;,&quot;taxRate&quot;: 0.21},  {&quot;id&quot;:&quot;courier&quot;,&quot;amount&quot;:0}]}"
    signature="DJNg/BEiWk6+9CR/wD6P2gbK1+LtAtB0wuqZEelc3tj22/8uVgiTLJ1bj6/H5AhMIiIzI7mIDAFTSc5l+BHZPM7Z/BAXU6mLrbqQUQ6apKmNcjDIDL+NQnZEVb3PCcLRO+IR5tRJLnTcJXVYBDwLlYfvK7INFLwz/4pvklGQOa/ZSHdMLsR5aHLpvfgm6gTlMszV63k8NxCgFghN6B6xdftcdQPskzEYXdpqn+++40bbYv3hReA1zaOCDupjrJirYcNP3P+f6V7t1MLkQqX2pqh3TJEkorrJH/0e1WMbHUdQY6NiJcsMN56qEntdQI6GiUQVNtwOj8sH9kMMdgdjbg==">
</klix-checkout>
```

Note that character `"` values are escaped in a JSON document passed as `order` attribute value.  

<!-- markdownlint-disable MD033 -->
<details>
    <summary>Expand to see an unescaped and formatted order attribute value</summary>
    ```json
    {
        "items":[
            {
                "orderItemId":"12345",
                "amount":88,
                "taxRate":0.21,
                "currency":"EUR",
                "label":"Vacuum cleaner TP-3",
            },
            {
                "amount":9.39,
                "currency":"EUR",
                "label":"TP-3 HEPA filter",
                "count":2,
                "unit":"PIECE"
            }
        ],
        "shippingOptions":[
            {
                "id":"omniva",
                "amount":2,
                "currency":"EUR",
                "taxRate":0.21
            },
            {
                "id":"courier",
                "amount":0
            }
        ]
    }
    ```
</details>
<!-- markdownlint-disable MD033 -->

Steps to calculate a signature:

1. Calculate signature input string. `Input string = CONCAT(widget ID, language, first order item amount, first order item currency, first order item label, first order item tax rate, first order item id, second order item amount, second order item currency, second order item label, second order item count, second order item unit, first shipping option id, first shipping option amount, first shipping option currency, second shipping option id, second shipping option amount) = 8c597447-5234-4de1-ab85-b54e252098celv88.00EURVacuum cleaner TP-30.21123459.39EURTP-3 HEPA filter2PIECEomniva2.00EURcourier0.00`. Note that fields without a value are not included in signature input string.

2. Calculate signature using private key previously downloaded from Klix Merchant Console and applying SHA256withRSA and Base64Encode functions to calculated input string. `Signature = Base64Encode(SHA256withRSA(signature-input)) = Base64Encode(SHA256withRSA("8c597447-5234-4de1-ab85-b54e252098celv88.00EURVacuum cleaner TP-30.21123459.39EURTP-3 HEPA filter2PIECEomniva2.00EURcourier0.00"))`.

Calculated signature which should be passed to Klix widget as a `signature` attribute value in this example: `DJNg/BEiWk6+9CR/wD6P2gbK1+LtAtB0wuqZEelc3tj22/8uVgiTLJ1bj6/H5AhMIiIzI7mIDAFTSc5l+BHZPM7Z/BAXU6mLrbqQUQ6apKmNcjDIDL+NQnZEVb3PCcLRO+IR5tRJLnTcJXVYBDwLlYfvK7INFLwz/4pvklGQOa/ZSHdMLsR5aHLpvfgm6gTlMszV63k8NxCgFghN6B6xdftcdQPskzEYXdpqn+++40bbYv3hReA1zaOCDupjrJirYcNP3P+f6V7t1MLkQqX2pqh3TJEkorrJH/0e1WMbHUdQY6NiJcsMN56qEntdQI6GiUQVNtwOj8sH9kMMdgdjbg==`.

## Klix Widget

### Klix Widget placement

Klix Widget is implemented as embeddable Web Component. In order to show Klix widget on a merchant website widget JavaScript should be included in the page and custom HTML tag needs to be placed in the required location. See the example below for integration code.

!!! Warning "Widget JavaScript location"
    Please note that Klix Widget JavaScript source should be loaded from different destination in case of production and test environments. Test environment Klix Widget JavaScript base path is  `https://klix.blob.core.windows.net/stage/widget/build/`. Production environment Klix Widget JavaScript base path is `https://klix.blob.core.windows.net/public/widget/build/`.

```html
<head>
    <script type="module" src="https://klix.blob.core.windows.net/stage/widget/build/klixwidget.esm.js">
    </script>
    <script nomodule src="https://klix.blob.core.windows.net/stage/widget/build/klixwidget.js">
    </script>
</head>
<body>
    KLIX embedded checkout:
    <klix-checkout language="lv" widget-id="604419cc-c313-4e47-93e8-330385669bdb">
    </klix-checkout>
</body>
```

Integration code can be accessed and copied from Widgets section of Merchant Console.

### Widget Configuration

Klix widget is created and configured in _Merchant Console_. Generated _Widget ID_ is used to embed Web Component into Merchant HTML page.

#### Specifying widget language

Widget language can be specified using attribute _language_ e.g. "lv" for Latvian, "en" for English or "ru" for Russian.

```html
<klix-checkout language="lv" widget-id="604419cc-c313-4e47-93e8-330385669bdb">
</klix-checkout>
```

#### Prefilling Klix widget form data

Klix widget form data prefilling allows to reduce a number of fields that needs to be entred by customer in Klix form in situations where required customer information was previously entered in merchant's webpage e.g. in case if Klix form is presented to exsiting merchant's customer or customer needs to fill specific form on merchant's page before continuing with Klix form.

Klix widget supports prefilling form with customer data specified as Klix widget attributes. Customer phone number, e-mail, first name and last name can be passed to widget:

```html
<klix-checkout widget-id="..." language="lv" email="john.doe@klix.app" phone="37120000000" first-name="John" last-name="Doe">
</klix-checkout>
```

Note that partial data prefill is supported i.e. if only firstname and last name of customer is passed to Klix widget it will be prefilled after non existing Klix customer enters his mobile phone number.

In case there's no Klix customer with specified mobile phone number Klix widget form will be prefilled with specified data.

![New customer data prefilled in widget](images/widget_new_customer_data_prefilled.png "New customer data prefilled in widget")

Otherwise Klix widget data autofill is triggered automatically so that customer receives autofill notifcation in his mobile phone.

![Existing customer data prefilled in widget](images/widget_existing_customer_data_prefilled.png "Existing customer data prefilled in widget")

### INSTANT (Fixed Price) Widget Type

It is possible to generate Klix widget for a specific order. In this case no validation on merchant side is required - all order items are defined at widget configuration in Merchant Console (MC). They can’t be overwritten by _Widget HTML API_ or _Widget JS API_ (specified later in the document) for security purposes. This type of Widget allows lightweight integration with merchant - no merchant back-end is required for integration.

Widget language is specified either in page _<html>_ tag or as a Klix widget attribute.

```html
<html lang="lv">
...
<klix-checkout widget-id="..."></klix-checkout>
```

OR

```html
<klix-checkout widget-id="..." language="lv"></klix-checkout>
```

### CHECKOUT (Dynamic Price) Widget Type

Order items should be provided either via _Widget HTML API_ (widget tag attributes) or via _Widget JS API_. Widget language is defined in the same manner as in _Fixed Price Widget_ type.

#### HTML API for single order item

Tax could be defined as rate (0.XX) or percentage (number greater than 1 without ‘%’ symbol).

Tax rate default value is merchant specific (e.g. for merchants registered in LV it would be "0.21"). Currency default value is merchant specific as well (e.g. for merchants registered in EU it would be "EUR").

All HTML widgets below are equivalent for merchant registered in LV:

```html
<klix-checkout widget-id="..." amount="1.99" currency="EUR" label="Philips XR3857" tax-rate="0.21" count="1" unit="PIECE"></klix-checkout>

<klix-checkout widget-id="..." amount="1.99" currency="EUR" label="Philips XR3857" tax-rate="21"></klix-checkout>

<klix-checkout widget-id="..." amount="1.99" currency="EUR" label="Philips XR3857"></klix-checkout>

<klix-checkout widget-id="..." amount="1.99" label="Philips XR3857"></klix-checkout>
```

#### HTML API for multiple order items

```html
<klix-checkout widget-id="..." order='{"items":[{"amount":40,"currency":"EUR","count":2,"unit":"PIECE","label":"Philips XR3857","taxRate":0.21},{"amount":5,"label":"Piegāde"}]}'>
</klix-checkout>
```

OR

```html
<klix-checkout widget-id="..." order="{&quot;items&quot;:[{&quot;amount&quot;:40,&quot;currency&quot;:&quot;EUR&quot;,&quot;count&quot;:2,&quot;label&quot;:&quot;Philips XR3857&quot;,&quot;taxRate&quot;:0.21},{&quot;amount&quot;:5,&quot;label&quot;:&quot;Piegāde&quot;}]}">
</klix-checkout>
```

Also product name value needs to be properly escaped (e.g. using `&amp;` for `&`, `&lt;` for `<`, `&#39;` for `'`, `&#34;` for `"`, etc.) to be a valid HTML attribute value.

##### Implementation details

```javascript
// merchant order
let orderObject = {
  "items": [
    {"amount":40,"currency":"EUR","label":"Philips XR3857","taxRate":0.21},
    {"amount":5,"label":"Piegāde"}
  ],
  "shippingOptions": [
    {"id":"pickup","amount":0},
    {"id":"omniva","amount":3,"currency":"EUR","taxRate":0.21},
    {"id":"latvijaspasts"}]
};

// this value goes into widget 'order' attribute
let orderAttribute = JSON.stringify(orderObject);

// this value goes into HTML attribute (e.g. client side templating)
let escaped = orderAttribute.split('&').join('&amp;').split('<')
  .join('&lt;').split("'").join('&#39;').split('"').join('&#34;');

// this value is received as widget internal state
let orderObjectInWidget = JSON.parse(orderAttribute);
```

#### JS API for single and multiple order items

Widget `'lazy'` attribute is used to delay widget initialization (until configuration via _Widget JS API_ is completed):

```html
<klix-checkout id="my-klix-checkout" widget-id="..." lazy="true"></klix-checkout>
...
<script>
  let checkoutWidget = document.querySelector('#my-klix-checkout');
  checkoutWidget.addOrderItem( {"amount":40,"currency":"EUR","label":"Philips XR3857","count":2,"unit":"PIECE","taxRate":0.21});
  checkoutWidget.addOrderItem({"amount":5,"label":"Piegāde"});

  // pick-up at store, free of charge 
  checkoutWidget.addShippingOption({"id":"pickup","amount":0});

  // shipping price is unknown, separate request will be initiated
  // for entered (by customer) address via Merchant API
  checkoutWidget.addShippingOption({"id":"latvijaspasts"});

  // shipping price (before tax) and tax rate are known for
  // given product/cart (whatever selected address or pickup point)
  checkoutWidget.addShippingOption({"id":"myIdForPickupProvider",
    "amount":5,"currency":"EUR","taxRate":0.21});
  
  checkoutWidget.init();
</script>
```

#### Order Item attributes

| Name        | Mandatory | Default value                      | Description                                          |
|-------------|-----------|------------------------------------|------------------------------------------------------|
| amount      | Yes       | n/a                                | Price per unit                                       |
| currency    | No        | Merchant specific (EUR in Latvia)  | Alphabetic code ISO 4217                             |
| label       | Yes       | n/a                                | Product or service name                              |
| count       | No        | 1                                  | Quantity                                             |
| unit        | No        | PIECE                              | {PIECE, KILOGRAM, METER, LITRE, HOUR}                |
| taxRate     | No        | Merchant specific (0.21 in Latvia) | VAT                                                  |
| orderItemId | No        | null                               | Product ID in merchant system for reference purposes |

### Javascript callbacks

Klix widget uses [Custom DOM events](https://developer.mozilla.org/en-US/docs/Web/Guide/Events/Creating_and_triggering_events) to communicate with merchant page. In order to listen specifc Klix widget event appropriate event listener should be registered first.

#### Payment completed callback

Event with type `paymentCompleted` is published in case of successfull or failed payment right before payment status screen is shown to customer.

![Klix widget payment status screen](images/widget_payment_status.png "Klix widget payment status screen")

Event handler can be used to dynamically update merchant page contents according to payment processing result.

```html
<klix-checkout widget-id="..." order="..."></klix-checkout>
<script>
    const checkoutElement = document.querySelector('klix-checkout');
    checkoutElement.addEventListener('paymentCompleted', event => {
        const message = 'Order with id ' + event.detail["orderId"] + ' payment succeeded -> ' + event.detail["paymentSucceeded"];
        window.alert(message);
        // Note that this callback can be used to hide Klix default payment success screen and show your own instead
        /*
        if (event.detail["paymentSucceeded"]) {
          checkoutElement.remove();  
          // display your own payment success message
        }
        */

    });
</script>
```

Following event properties can be used:

| Property                         | Type    | Description                              |
|----------------------------------|-------- |------------------------------------------|
| event.detail["orderId"]          | string  | Completed payment order identifier       |
| event.detail["paymentSucceeded"] | boolean | Indicates if payment succeeded or failed |

#### Navigate back to payment method selection / shopping cart page

Event with type `navigateBackToCart` is published in case if customer presses "Back to merchant store" button on cancelled/failed payment Klix widget form.

<!-- markdownlint-disable MD033 -->
<p style="text-align: center;">
  <img src="../images/widget_failed_payment.png" width="500" alt="Failed/cancelled payment screen" />
</p>

Event handler should point browser to checkout or payment method selection page so that customer can use another payment method if available.

```html
<klix-checkout widget-id="..." order="..."></klix-checkout>
<script>
    const checkoutElement = document.querySelector('klix-checkout');
    checkoutElement.addEventListener('navigateBackToCart', () => {
        location.pathname = '/shopping-cart';
    });
</script>
```

### Limiting accepted cards

By default all cards issued by Mastercard and VISA are accepted by Klix however following constraints can be imposed via order configuration if needed:

| Constraint                       | Configuration example                          | Description
|----------------------------------|------------------------------------------------|----------------------------------------------------
| Accepted card issuer             | ```"constraints": {"issuer": "Citadele"}```    | Only cards issued by Citadele bank are accepted for this payment
| Accepted card brand              | ```"constraints": {"brand": "X karte"}```      | Only Citadele "X karte" brand cards are accepted for this payment
| Accepted card payment scheme     | ```"constraints": {"paymentScheme": "VISA"}``` | Only VISA cards are accepted for this payment

```html
<klix-checkout widget-id="..." order="{&quot;constraints&quot;:{&quot;issuer&quot;:&quot;Citadele&quot;},&quot;items&quot;:[{&quot;amount&quot;:40,&quot;currency&quot;:&quot;EUR&quot;,&quot;count&quot;:2,&quot;label&quot;:&quot;Philips XR3857&quot;,&quot;taxRate&quot;:0.21}]}">
</klix-checkout>
```

## Client libraries

Klix team maintains official API client libraries which wraps Klix API invocations including request signing and response signature validation.

If you have implemented client library for programming language or popular framework that's not listed here then let us know it so that we might share it with others.

### Available libraries

#### PHP

* [Github](https://github.com/klix-app/merchant-api-php)
* [Packagist](https://packagist.org/packages/klix-app/merchant-api-php)

## Integration testing

### Environments

Klix provides a fully functional test environment that can be used to test Klix integration implementation without a need to use real bank cards or make actual transactions. Follow instructions in the [step by step guide](#step-by-step-integration-instructions) to receive access to test both Merchant Console and API.

| Service                     | Test environment                                                 | Production environment                                            |
|-----------------------------|------------------------------------------------------------------|-------------------------------------------------------------------|
| Merchant Console           | <sub>https://mc.stage.klix.app</sub>                             | <sub>https://mc.klix.app</sub>                                    |
| Merchant API                | <sub>https://api.stage.klix.app</sub>                            | <sub>https://api.klix.app</sub>                                   |
| Widget Javascript base path | <sub>https://klix.blob.core.windows.net/stage/widget/build/</sub>| <sub>https://klix.blob.core.windows.net/public/widget/build/</sub>|

### Test environment bank cards

Test environment is not connected to real bank card issuer networks therefore only special test cards can be used in test environment.

| Issuer     | PAN                 | CVV | Expiry date | 3D secure password |
|------------|---------------------|-----|-------------|--------------------|
| VISA       | 4314 2200 0000 0056 | 123 | 01/22       | hint               |
| MASTERCARD | 5413 3300 0000 0019 | 589 | 01/22       | hint               |

Note that any cardholder name can be used with these cards.

## Merchant API v2

Klix provides merchant API that can be used to perform various operations with orders e.g. retrieve order by identifier, fully or partially refund an order payment transaction, etc. Note that Klix currently does not provide an API to make a payment without interacting with Klix Widget.

### Swagger documentation

Up to date Klix Merchant API [Swagger](https://swagger.io/) documentation is available [here](https://api.stage.klix.app/v2/merchants) and can be previewed using Swagger Inspector [online](https://inspector.swagger.io/builder?url=https%3A%2F%2Fapi.stage.klix.app%2Fv2%2Fmerchants).

### Authentication

#### Authentication method

All requests sent to Klix Merchant API should be digitally signed using merchant certificate generated in Klix Merchant Console. Signing process is performed according to ["Draft cavage HTTP Signatures"](https://www.ietf.org/archive/id/draft-cavage-http-signatures-12.txt) specification.
Request signature is sent as an HTTP `Authorization` header value together with metadata describing signature algorithm and certificate key used.

#### Signing requests without request body

The following HTTP headers should be sent for all requests without a request body: Authorization, Date.
Signed HTTP GET request example:

```sh
curl -X GET \
  https://api.stage.klix.app/v2/merchants/052eb290-a6ca-41f0-aac5-fd4ca14632e3/orders/a6f0f3c7-e3e7-4182-bc4c-bddec2a89d9a \
  -H 'Authorization: Signature keyId="ad537135-59f5-4073-8f8e-1137fc19b9d2",algorithm="rsa-sha256",headers="date (request-target)",signature="xkdqOeB0Iy4iaSQRgIWsyCuSewMzOcDISMdKO6ThFiRrvZXxdVnW1xY6WZH8E3y2qdyazSUOfMuPstVQE1ATLdUDeZFiCgRO4iYcjEviSSsXQuKT3agtad2qg8pza8w2rCmfuCjFZ87ntPKFfYqMkNTHkZB4EM/l+JEDwRXiOhRTfyVQU/xRzzq+PQ3rzSAGAPceoAmzIp6QgRVZDDAO67ktYVJ2Y9qfKGBggtQwXkZPsDy622zP8hODYLqUQlL3qsAgOL6a0RlqpOKFWJZEt4ZHfY3qBxHFUhlMyQfExZyZXMOrt4/SNNPNJwUkNbHLYXVVRuGGh2Wmc78vP464Tw=="' \
  -H 'date: Fri, 15 May 2020 10:28:40 GMT'
```

Note that HTTP header `Authorization` starts with `Signature` and thus specifies that HTTP Signatures authentication type should be used. Signature metadata description is summarized in a following table:

| Field  name          | Sample value                         | Description                                                                                 |
|----------------------|--------------------------------------|---------------------------------------------------------------------------------------------|
| keyId                | ad537135-59f5-4073-8f8e-1137fc19b9d2 | Merchant certificate name that can be found in Certificates section in Klix Merchant Console|
| algorithm            | rsa-sha256                           | Encryption algorithm used. Supported algorithms: rsa-sha256, rsa-sha384, rsa-sh512          |
| headers              | date (request-target)                | HTTP headers that are included in signature. For HTTP requests without body at least `date` and `request-target` headers should be included. Field `(request-target)` is not a HTTP header but rather a combination of HTTP method and request URI e.g. in this case `get /v2/merchants/052eb290-a6ca-41f0-aac5-fd4ca14632e3/orders/a6f0f3c7-e3e7-4182-bc4c-bddec2a89d9a`. In case additional HTTP headers are included in signature these fields should also be added to headers list |

#### Signing requests with request body

The following HTTP headers should be sent for all requests with a request body: Authorization, Date, Content-Type, Digest.
Signed HTTP POST request example:

```sh
curl -X POST \
  https://api.stage.klix.app/v2/merchants/052eb290-a6ca-41f0-aac5-fd4ca14632e3/orders/a6f0f3c7-e3e7-4182-bc4c-bddec2a89d9a/refunds \
  -H 'Authorization: Signature keyId="ad537135-59f5-4073-8f8e-1137fc19b9d2",algorithm="rsa-sha256",headers="digest date content-type (request-target)",signature="xkdqOeB0Iy4iaSQRgIWsyCuSewMzOcDISMdKO6ThFiRrvZXxdVnW1xY6WZH8E3y2qdyazSUOfMuPstVQE1ATLdUDeZFiCgRO4iYcjEviSSsXQuKT3agtad2qg8pza8w2rCmfuCjFZ87ntPKFfYqMkNTHkZB4EM/l+JEDwRXiOhRTfyVQU/xRzzq+PQ3rzSAGAPceoAmzIp6QgRVZDDAO67ktYVJ2Y9qfKGBggtQwXkZPsDy622zP8hODYLqUQlL3qsAgOL6a0RlqpOKFWJZEt4ZHfY3qBxHFUhlMyQfExZyZXMOrt4/SNNPNJwUkNbHLYXVVRuGGh2Wmc78vP464Tw=="' \
  -H 'Content-Type: application/json' \
  -H 'content-length: 130' \
  -H 'date: Fri, 15 May 2020 10:28:40 GMT' \
  -H 'digest: sha-256=HoohlqpLJgeDLYagaolWSlbRC7/eMwK1O9TwgT27DzU=' \
  -d '{
    "refundRequest": {
        "amount": 5.00,
        "reason": "OTHER_REFUND",
        "note": "Internal note visible in Merchant Console"
    }
}'
```

Signature metadata description that differs from HTTP requests without request body is summarized in a following table:

| Field  name          | Sample value                                         | Description                                                                                 |
|----------------------|------------------------------------------------------|---------------------------------------------------------------------------------------------|
| headers              | digest date content-type (request-target)            | HTTP headers that are included in signature. For HTTP requests with body at least `digest`, `date`, `content-type` and `request-target` headers should be included. Field `(request-target)` is not a HTTP header but rather a combination of HTTP method and request URI e.g. in this case `post /v2/merchants/052eb290-a6ca-41f0-aac5-fd4ca14632e3/orders/a6f0f3c7-e3e7-4182-bc4c-bddec2a89d9a/refunds`. In case additional HTTP headers are included in signature these fields should also be added to headers list |

HTTP header `digest` consists of HTTP request body digest algorithm and digest value seperated by `=` sign i.e. `sha-256=base64(sha256(request body))`. Supported digest algorithms are SHA-256, SHA-384 and SHA-512. See [additional details](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Digest).

#### Libraries

Here is a list of libraries that can be used to simplify merchant authentication in Klix using HTTP Signatures specification. Note that this is not an extensive list and there might be a better candidate for programming language and framework used in your project.

| Programming language | Library                                                                                     |
|----------------------|---------------------------------------------------------------------------------------------|
| Go                   | [99designs/httpsignatures-go](https://github.com/99designs/httpsignatures-go)               |
| Java                 | [tomitribe/http-signatures-java](https://github.com/tomitribe/http-signatures-java)         |
| node.js              | [joyent/node-http-signature](https://github.com/joyent/node-http-signature)                 |
| PHP                  | [liamdennehy/http-signatures-php](https://github.com/liamdennehy/http-signatures-php)       |
| Ruby                 | [99designs/http-signatures-ruby](https://github.com/99designs/http-signatures-ruby)         |
