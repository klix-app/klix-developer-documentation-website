# Supported payment methods

Klix currently supports following payment methods:

| Name                 | Description             | 
|----------------------|-------------------------|
| klix                 | Klix card payments      |
| citadele_ee_digilink | Citadele Bank payments  |
| citadele_lt_digilink | Citadele Bank payments  |
| citadele_lv_digilink | Citadele Bank payments  |
| coop_pank_ee_pis     | Coop Pank payments      |
| klix_apple_pay       | Apple Pay payments      |
| klix_google_pay      | Google Pay™ payments    |
| klix_pay_later       | Klix Pay Later          |
| lhv_ee_pis           | LHV payments            |
| luminor_ee_pis       | Luminor payments        |
| luminor_lt_pis       | Luminor payments        |
| luminor_lv_pis       | Luminor payments        |
| revolut_pis          | Revolut payments        |
| seb_ee_pis           | SEB payments            |
| seb_lt_pis           | SEB payments            |
| seb_lv_pis           | SEB payments            |
| siauliu_lt_pis       | Šiaulių Bankas payments |
| swedbank_ee_pis      | Swedbank payments       |
| swedbank_lt_pis      | Swedbank payments       |
| swedbank_lv_pis      | Swedbank payments       |

## Payment method availability

Note that [available payment method API end-point](../api/#get-list-of-available-payment-methods) returns all payment methods that are enabled for you even if end-user's device/browser does not support particular payment method. This section describes the additional checks that should be done for certain payment methods to determine if they are available in customer's device/browser.

## Apple Pay

Use following conditions to detect if customer's device/browser supports Apple Pay payments:

```html
<script type="text/javascript">
    if (window.ApplePaySession && ApplePaySession.canMakePayments()) {
        // customer's device & browser supports Apple Pay -> Apple Pay payment button should be included in payment method list
    }
</script>
```

## Google Pay™

Use following conditions to detect if customer's device/browser supports Google Pay payments:

```html
<script async src="https://pay.google.com/gp/p/js/pay.js" onload="onGooglePayLoaded()"></script>
<script type="text/javascript">
    function onGooglePayLoaded() {
    const paymentsClient = new google.payments.api.PaymentsClient({
        environment: 'PRODUCTION'
    });
    const googleIsReadyToPayRequest = {
        apiVersion: 2,
        apiVersionMinor: 0,
        allowedPaymentMethods: [
            {
                tokenizationSpecification: {
                    type: 'PAYMENT_GATEWAY'
                },
                type: "CARD",
                parameters: {
                    allowedAuthMethods: ["PAN_ONLY", "CRYPTOGRAM_3DS"],
                    allowedCardNetworks: ["VISA", "MASTERCARD"]
                }
            }
        ]
    };
    paymentsClient.isReadyToPay(googleIsReadyToPayRequest)
        .then(function(response) {
            if (response.result) {
                // customer's device & browser supports Google Pay -> Google Pay payment button should be included in payment method list
            }
        });
    }
</script>
```