# Supported payment methods

Klix currently supports following payment methods:

| Name                  | Description                 | Supports personal ID validation | Supports full name validation |
|-----------------------|-----------------------------|---------------------------------|-------------------------------|
| citadele_ee_digilink  | Citadele Bank payments      |                                 |               X               |
| citadele_lt_digilink  | Citadele Bank payments      |                                 |               X               |
| citadele_lv_digilink  | Citadele Bank payments      |                                 |               X               |
| coop_pank_ee_pis      | Coop Pank payments          |                                 |                               |
 | indexo_lv_pis         | INDEXO bank payments        |                                 |                               |
| klix                  | Klix card payments          |                                 |                               |
| klix_apple_pay        | Apple Pay payments          |                                 |                               |
| klix_google_pay       | Google Pay™ payments        |                                 |                               |
| klix_pay_later        | Klix Pay Later              |                                 |                               |
| lhv_ee_pis            | LHV payments                |                                 |                               |
| luminor_ee_pis        | Luminor payments            |                X                |                               |
| luminor_lt_pis        | Luminor payments            |                X                |                               |
| luminor_lv_pis        | Luminor payments            |                X                |                               |
| revolut_pis           | Revolut payments            |                                 |               X               |
| seb_ee_pis            | SEB payments                |                X                |                               |
| seb_lt_pis            | SEB payments                |                X                |                               |
| seb_lv_pis            | SEB payments                |                X                |                               |
| siauliu_lt_pis        | Šiaulių Bankas payments     |                                 |                               |
| swedbank_ee_pis       | Swedbank payments           |                X                |                               |
| swedbank_lt_pis       | Swedbank payments           |                X                |                               |
| swedbank_lv_pis       | Swedbank payments           |                X                |                               |

## Payment method specifics

Note that [available payment method API end-point](../api/#get-list-of-available-payment-methods) returns all payment methods that are enabled for you even if end-user's device/browser does not support particular payment method. This section describes the specifics of particular payment methods including additional checks that should be done in order to determine if particular payment method is available in customer's device/browser.

### Apple Pay

#### Checking Apple Pay support in device/browser

Use following conditions to detect if customer's device/browser supports Apple Pay payments:

```html
<script type="text/javascript">
    if (window.ApplePaySession && ApplePaySession.canMakePayments()) {
        // customer's device & browser supports Apple Pay -> Apple Pay payment button should be included in payment method list
    }
</script>
```

### Google Pay™

!!! Note "All merchants must adhere to the Google Pay APIs [Acceptable Use Policy](https://payments.developers.google.com/terms/aup) and accept the terms defined in the [Google Pay API Terms of Service](https://payments.developers.google.com/terms/sellertos)."

If you have selected Google Pay as one of the payment methods in Klix agreement then no additional actions should be made to enable Google Pay payments for your merchant account. Google Pay is available in a redirect flow as any other payment method provided by Klix. Klix takes care of providing correct parameters to Google Pay SDK and only case you'll need to interact with Google Pay SDK directly is in case you decide to display Klix payment method selection directly in your checkout page and would like to hide Google Pay button in case it's not supported by customer's device/browser. In such case please follow instructions provided in section [Checking Google Pay support in device/browser](#checking-google-pay-support-in-devicebrowser).

#### 3DS support

For PAN_ONLY card transaction authentication (this authentication method is associated with payment cards stored on file with the user's Google Account) Klix will request 3DS authentication just as for any regular card transaction processed by Klix. No additional actions should be performed in order to request 3DS for PAN_ONLY transactions and there's no option to disable 3DS for PAN_ONLY transactions.

#### Checking Google Pay support in device/browser

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