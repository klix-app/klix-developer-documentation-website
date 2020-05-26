# Quick Start guide

 This 5 minute guide covers overview of Klix integration technical aspects.

## Integration steps

A central part of Klix checkout/payment solution is Klix web widget - HTML web component i.e. form for entering customer's credit card data. Klix widget accepts merchant order parameters (order identifier, amount, payment description, etc.) via HTML element attributes. The following steps describe actions that you need to perform in order to add Klix widget to your webshop and accept a callback on a server-side once payment has been successfully processed by Klix.

### 1. Embed Klix widget into your web shop

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

Note that Klix widget JavaScript code should be loaded from different destinations on production and test environments. See Testing integration [Environments](/../testing-integration/#Environments) section for details.

### 2. Pass order information to Klix widget

Next step is to place Klix widget HTML code in an appropriate place on a checkout page where this widget will be rendered and pass order/payment relarted information.

```html
<klix-checkout widget-id="21ca7904-ff16-48b5-918d-c2d80af81f05"  
    amount="5.45"  
    currency="EUR"  
    label="Order No 12345678"  
    language="lv"
    signature="B+nre6Oe6lnjh0hcW5dhOtRmXxN3pm6Sup3kjcNeQiSmTN6zQCp6kHErX/s+JIvkLIqQxD2D/EU2MUraQC03RyKHyX/Wr8qVVbPeBaskPkYR7l397BBYOghvVN1LS8RWdpQ4Q67kMYdPutqnJAUGJtHA51i14xmnaIRxctpK4UJE3qtfu1QjWPez/yP1lT/igpCTL66lqXKcbHac75v++5WUwwT5fCEUklPxudzC3qbujNhXZBPwAZxa2GaYQDzCOP7p/bcJgH/DwsaVMiDtekG5ANgXB51WOPB9X3pP1rdr6kbVccXhN0D4UrxMt3ZA4bPw+LaAWzVRNaVOJoNpZg==">
</klix-checkout>
```

Note that field `signature` should contain valid order signature. See section [Signing order](../security/#signing-order) for detailed instructions on how to generate a valid signature.
Here's Klix widget that corresponds to previously mentioned HTML code.

<!-- markdownlint-disable MD033 -->
<details>
    <summary>Click to load Klix widget</summary>

<div>
    <klix-checkout widget-id="21ca7904-ff16-48b5-918d-c2d80af81f05" amount="5.45" currency="EUR" label="Order No 12345678" language="lv" signature="B+nre6Oe6lnjh0hcW5dhOtRmXxN3pm6Sup3kjcNeQiSmTN6zQCp6kHErX/s+JIvkLIqQxD2D/EU2MUraQC03RyKHyX/Wr8qVVbPeBaskPkYR7l397BBYOghvVN1LS8RWdpQ4Q67kMYdPutqnJAUGJtHA51i14xmnaIRxctpK4UJE3qtfu1QjWPez/yP1lT/igpCTL66lqXKcbHac75v++5WUwwT5fCEUklPxudzC3qbujNhXZBPwAZxa2GaYQDzCOP7p/bcJgH/DwsaVMiDtekG5ANgXB51WOPB9X3pP1rdr6kbVccXhN0D4UrxMt3ZA4bPw+LaAWzVRNaVOJoNpZg=="></klix-checkout>
</div>
</details>
<!-- markdownlint-disable MD033 -->

### 3. Implement an end-point that will be invoked upon payment completetion

After payment is completed (both successfully or failed) Klix server will send a callback HTTP request to your API end-point defined in Merchant Console. See [Purchase notification HTTP request example](/callbacks/#purchase-notification-request-example).
Note that first thing upon receiving purchase completed HTTP request you should verify request signature in order to ensure that request was sent by Klix server. See [Callback payload signature validation](/callbacks/#callback-payload-signature-validation) for details.

## Next steps

Continue with [Step by step](../step-by-step/) Klix integration and configuration instructions.
