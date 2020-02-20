# Callbacks

## Callback URLs

During order lifecycle Klix invokes API end-points implemented by merchant store. All URLs should be implemented and accessible via HTTPS in order to be callable by Klix.

| URL Type                      | Example                            | Description
|-------------------------------|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Terms & Conditions URL        | https://merchant.com/terms.html    | HTTP GET request is sent from user device (browser or mobile) to retrieve merchant legally binding agreement text.    |
| Merchant Callback URL         | https://merchant.com/callbacks     | Connection test end-point. HTTP OPTIONS request is sent from Klix backend to check if HTTP connection to merchant server can be established. This end-point should return HTTP status 200 and header Access-Control-Allow-Methods should contain POST method.|
| Order verification URL        | https://merchant.com/orders        | Deprecated - used only in case of Klix widgets with valiation type `CALLBACK`. HTTP POST request is sent from Klix backend to this end-point whenever a customer submits an order in the checkout form. Merchant store should validate order data (check if selected product/service price is valid and is available in stock etc.) and either approve or reject an order. Note that Klix does not send full order information in notification request body. Only order identifier is sent as a JWS payload. You should check JWS signature, Base64 decode request payload to extract order identifier and use Klix API to retrieve order data by order identifier. Example JWS sent as a notification body: `eyJraWQiOiJNUGF5IiwiYWxnIjoiUlMyNTYifQ.eyJvcmRlcklkIjoiMWE2YTUzNjgtZTc5OS00OTY3LWI3NDItNjdhZGMxNmFlYzhhIn0.OZQU_2nPKUWl93U8YJJ9GjzZlLmmKS7ffuVu1WSZ5Y4JSU65LJxYs3kj0a460abXsBLkkVGX1Hi89fxNJ8CMsQ`. Base64 decoding middle part of this JWS token will lead to following JSON document `{"orderId":"1a6a5368-e799-4967-b742-67adc16aec8a","externalOrderId":"1233456789"}`, where `orderId` is Klix order identifier and `externalOrderId` is order identifier in merchant's system.
| Purchase notification URL     | https://merchant.com/purchases     | HTTP POST request is sent from Klix backend to confirm that payment has either been collected successfully on behalf of the merchant or either payment has failed/was cancelled. Note that error code and message might not be present even in case of failed payment. See [Purchase notification request example](#purchase-notification-example).|
| Shipping cost calculation URL | https://merchant.com/shipping-cost | HTTP POST request is sent from Klix backend in order to calculate shipping costs for certain order. Note that this end-point should be implemented only in case merchant supports dynamic pricing delivery i.e. delivery price can be determined only after Klix client has entered delivery address. Otherwise different delivery option cost can be passed as Klix [Widget](../widget/) parameter. See [Shipping cost calculation request example](#shipping-cost-calculation-request-jws-payload-example).|

### Purchase notification request example

HTTP status code 200 should be returned by this API end-point otherwise Klix server will retry to send notification multiple times.

```bash
#!/bin/bash
curl -x POST https://your.site/payment-notifications \
    -H 'Content-Type: application/json' \
    -H 'X-Klix-Signature: TODO' \
    -d '{
        "TODO": "Example here"
    }'
```

### Shipping cost calculation request example

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

## Updating callback URLs

URLs can be set in Merchant Console settings.

![alt_text](images/callback_urls.png "Callback URLs configuration in Merchant Console")

## Callback payload signature validation

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

    return openssl_verify($payload, base64_decode($signature_header_value), 
        $klix_public_key, OPENSSL_ALGO_SHA256);
}
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
