# Steo by step integration instructions

## 1. Get access to Klix Merchant Console

Klix Merchant Console is a merchant self-service web-page for merchant profile and order data management.
In order to receive access to Merchant Console contact [support@klix.app](mailto:support@klix.app?subject=Merchant%20onboarding%20request&body=Hello.%0D%0AWe would like to request access to Klix checkout solution.%0D%0AKlix Merchant Console and API will be accessed from the following IP addresses on the test environment: PLEASE_REPLACE_THIS_WITH_IP_ADDESS%0D%0AE-mail address to access Merchant Console: PLEASE_REPLACE_THIS_WITH_EMAIL_ADDRESS) and request merchant onboarding in Klix.
Make sure to specify IP address from which Klix solution will be accessed and e-mail address for logging into Merchant Console. Merchant Console invitation e-mail will be sent after the request has been reviewed.

## 2. Decide which integration scenario suits you best

There are three types of integration scenarios currently supported by Klix. Choose one depending on a type of your existing webshop:

1. Simple static merchant website integration. For each product separate Klix Instant Widget should be created and widget's HTML code should be placed on merchant's product page. In such case only specific product(s) can be purchased using specific widget and for each product(s) a separate widget should be created. Orders and payments are fully managed in Klix Merchant Console. Therefore this integration type is suitable for rather small product catalog (up to 10 - 15 products).
![Simple integration using instant widget](images/instant_widget.png "Instant widget")

2. Klix Pay. Klix widget is integrated into custom built or standard e-commerce platform webshop and is used just as a payment option. In such case webshop's existing checkout page is responsible for collecting customer's information including delivery addressw. It means that Klix  Single Klix Checkout Widget can be created to handle multi-product shopping carts and unlimited product catalogs. This requires [callbacks](../callbacks/) implementation in merchant's web store.

    <!-- markdownlint-disable MD033 -->
    <details>
    <summary>Klix Pay example</summary>

    <klix-checkout widget-id="21ca7904-ff16-48b5-918d-c2d80af81f05" amount="5.45" currency="EUR" label="Order No 12345678" language="lv" signature="B+nre6Oe6lnjh0hcW5dhOtRmXxN3pm6Sup3kjcNeQiSmTN6zQCp6kHErX/s+JIvkLIqQxD2D/EU2MUraQC03RyKHyX/Wr8qVVbPeBaskPkYR7l397BBYOghvVN1LS8RWdpQ4Q67kMYdPutqnJAUGJtHA51i14xmnaIRxctpK4UJE3qtfu1QjWPez/yP1lT/igpCTL66lqXKcbHac75v++5WUwwT5fCEUklPxudzC3qbujNhXZBPwAZxa2GaYQDzCOP7p/bcJgH/DwsaVMiDtekG5ANgXB51WOPB9X3pP1rdr6kbVccXhN0D4UrxMt3ZA4bPw+LaAWzVRNaVOJoNpZg=="></klix-checkout>

    </details>
    <!-- markdownlint-disable MD033 -->

3. Klix Checkout. Just as in Klix Pay scenario Klix widget is integrated into custom built or standard e-commerce platform webshop but in this case Klix is used as a full checkout solution. It means that order information is still maintained in merchant's webshop and during checkout process order information is passed to Klix wiget which is responsible for collecting customer's information including delivery address. Same as in Klix Pay scenario both after successful and failed payment merchant's [callback](../callbacks/) is invoked by passing both order, customer and delivery information.

    <!-- markdownlint-disable MD033 -->
    <details>
    <summary>Klix Checkout example</summary>

    <klix-checkout widget-id="8c597447-5234-4de1-ab85-b54e252098ce" language="lv" order="{&quot;items&quot;: [{&quot;orderItemId&quot;: &quot;12345&quot;, &quot;amount&quot;: 88, &quot;taxRate&quot;: 0.21, &quot;currency&quot;: &quot;EUR&quot;, &quot;label&quot;: &quot;Vacuum cleaner TP-3&quot;}, {&quot;amount&quot;: 9.39, &quot;currency&quot;: &quot;EUR&quot;, &quot;label&quot;: &quot;TP-3 HEPA filter&quot;,&quot;count&quot;: 2, &quot;unit&quot;: &quot;PIECE&quot;}], &quot;shippingOptions&quot;: [{&quot;id&quot;:&quot;omniva&quot;,&quot;amount&quot;:2,&quot;currency&quot;:&quot;EUR&quot;,&quot;taxRate&quot;: 0.21},  {&quot;id&quot;:&quot;courier&quot;,&quot;amount&quot;:0}]}" signature="vy1KbYGxD2oZtSGvkoVLt8ZdHCkecbFpK9L7lsyK01D6FNvzTg3L3XF894sX68mmmPBm3BjMdw8QPsbz4d68aJu6WKj9hk5qu2sPmXKyt7ZgIVdHZJwI729g+Z5MFHDfmqbG1JBlQSYpMRrA1NX8zi1d4/+Sono1huvuc0H422BrxUt//CJSfug91mp/ZSkb7q5KiPZaZBpPHyNtNSuwvHUc5WO5KX2En0j4+8q/O3ekZEEDa5dsdhG4G2nZdb3ti8Wa4DQ+NTYmQ170QVoWtQwlleJ7MbP03K2kxikUqhEShLGfKkoFuhtkih0654ZnsG7AYs7ga8WvXEZ7SB2DHQ=="></klix-checkout>

</details>
<!-- markdownlint-disable MD033 -->

## 3. Provide integration configuration in Merchant Console

Follow registration link in Merchant Console invitation e-mail and specify new password. After registration follow [these steps](../configuration/) to configure integration.

## 4. Add widget HTML code to your web-page

Add Klix widget HTML code to your product or checkout page depending on a type of used widget (instant, checkout, payment) and your webshop specifics. More information about widget configuration can be found on [Widget](../widget/) page.

## 5. Implement purchase completed callback end-point

[Callback](../callbacks/) end-point should be implemented so that Klix can send payment status to merchant.

## 6. Implement callbacks to navigate back to payment method selection page or display additional message in case of successfull payment

JavaScript [callaback](../widget/#navigate-back-to-payment-method-selection-shopping-cart-page) should be implemented in order to allow customer to navigate back to payment method selection page or checkout page in case of failed or cancelled Klix payment.
Additional [callback](..widget/#payment-completed-callback) can be implemented in order to adjust payment status page depending on a payment result.

## 7. Test integration

Klix provides fully functional test environment that can be used to perform end to end [integration implementation testing](../testing-integration/).

## 8. Sign agreement and request access to production environment

After integration has been successfully tested on test environment you can proceed with creadentials request for production.
