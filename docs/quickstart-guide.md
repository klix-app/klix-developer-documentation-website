# Quick Start guide

 This 5 minute guide describes how to integrate Klix into your web shop.

## 1. Embed Klix widget into your web shop

In order to embed Klix checkout/pay widget into your web shop following HTML fragment should be added to your web shop checkout page.

```html
<head>
    <script type="module"  
        src="https://klix.blob.core.windows.net/public/widget/build/klixwidget.esm.js">
    </script>
    <script nomodule  
        src="https://klix.blob.core.windows.net/public/widget/build/klixwidget.js">
    </script>
</head>
```

This will load Klix widget JavaScript code. Note that Klix widget JavaScript code should be loaded from different destinations on production and test environments. See Testing integration [Environments](/../testing-integration/#Environments) section for details.

## 2. Pass order information to Klix widget

Next step is to place Klix widget HTML code in an appropriate place on a checkout page where this widget will be rendered and pass order/payment relarted information.

```html
<checkout-widget widget-id="21ca7904-ff16-48b5-918d-c2d80af81f05"  
    language="lv"  
    amount="5.45"  
    currency="EUR"  
    label="Order No 12345678"  
    tax-rate="0.21"  
    count="1"  
    unit="PIECE"  
    signature="">
</checkout-widget>
```

Note that field `signature` should contain valid order signature. See section [Signing order](../signing-order/) for detailed instructions on how to generate a valid signature.
Here's Klix widget that corresponds to previously mentioned HTML code.

<!-- markdownlint-disable MD033 -->
<div>
    <checkout-widget widget-id="21ca7904-ff16-48b5-918d-c2d80af81f05" language="lv" amount="5.45" currency="EUR" label="Philips XR3857" tax-rate="0.21" count="1" unit="PIECE" signature=""></checkout-widget>
</div>
<!-- markdownlint-disable MD033 -->

## 3. Implement an end-point that will be invoked upon payment completetion

After payment is completed (both successfully or cancelled) Klix server will send a callback HTTP request to your API end-point defined in Merchant Console. See [Integration configuration](../configuration/) section for details.
Here's HTTP request example that will be sent to your API end-point.

```bash
#!/bin/bash
curl -x POST https://your.site/payment-notifications \
    -H 'Content-Type: application/json' \
    -H 'X-Klix-Signature: TODO' \
    -d '{
        "todo": "Example here"
    }'
```

HTTP status code 200 should be returned by your API end-point otherwise Klix server will retry to send notification multiple times.
Note that first thing upon receiving purchase completed HTTP request you should verify request signature in order to ensure that request was sent by Klix server. Signature is sent as HTTP header `X-Klix-Signature` value and should be verified using `SHA256WithRSA` algorithm and Klix public key that can be downloaded from Merchant Console. Example signature validation code:

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
