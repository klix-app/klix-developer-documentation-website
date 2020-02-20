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

    <klix-checkout widget-id="21ca7904-ff16-48b5-918d-c2d80af81f05" amount="5.45" currency="EUR" order-id="12345678" label="Order No 12345678" language="lv" signature="n0b7Sj0qEVlU52kNctEHR9zUZ9pRtPjA/5/avPSQamx7HiI3q6qijgstBw6KhOKZqcCIL3RULbWNu6xoSGnNtW/nx+RcSd12I0st21Los9MXXPakEIjIto+2Zx0+ZiVoa97dxO8/iGF5A1U4qW9GFhPJGPqQecmZSp7rYaiO+VRq5D9KqKqRpBQEYN9YJgDgWMn36KVYkTdlOYAhJslwkVeKKZ+/ifUqHhiXbPKD3VKAXwx7/MqSiRlfU8Qsm7Vcv/zV05X9trZiaSYOL6yd9aWO/KE2so2hAswY58i6dR218/XD6ab5xTpCSXrfjYbfhInchukvlH7CrbE1T3RcWw=="></klix-checkout>

    </details>
    <!-- markdownlint-disable MD033 -->

3. Klix Checkout. Just as in Klix Pay scenario Klix widget is integrated into custom built or standard e-commerce platform webshop but in this case Klix is used as a full checkout solution. It means that order information is still maintained in merchant's webshop and during checkout process order information is passed to Klix wiget which is responsible for collecting customer's information including delivery address. Same as in Klix Pay scenario both after successful and failed payment merchant's [callback](../callbacks/) is invoked by passing both order, customer and delivery information.

    <!-- markdownlint-disable MD033 -->
    <details>
    <summary>Klix Checkout example</summary>

    <klix-checkout widget-id="8c597447-5234-4de1-ab85-b54e252098ce" language="lv" order="{&quot;items&quot;: [{&quot;orderItemId&quot;: &quot;12345&quot;, &quot;amount&quot;: 88, &quot;taxRate&quot;: 0.21, &quot;currency&quot;: &quot;EUR&quot;, &quot;label&quot;: &quot;Vacuum cleaner TP-3&quot;}, {&quot;amount&quot;: 9.39, &quot;currency&quot;: &quot;EUR&quot;, &quot;label&quot;: &quot;TP-3 HEPA filter&quot;,&quot;count&quot;: 2, &quot;unit&quot;: &quot;PIECE&quot;}], &quot;shippingOptions&quot;: [{&quot;id&quot;:&quot;omniva&quot;,&quot;amount&quot;:2,&quot;currency&quot;:&quot;EUR&quot;,&quot;taxRate&quot;: 0.21},  {&quot;id&quot;:&quot;courier&quot;,&quot;amount&quot;:0}]}" signature="DJNg/BEiWk6+9CR/wD6P2gbK1+LtAtB0wuqZEelc3tj22/8uVgiTLJ1bj6/H5AhMIiIzI7mIDAFTSc5l+BHZPM7Z/BAXU6mLrbqQUQ6apKmNcjDIDL+NQnZEVb3PCcLRO+IR5tRJLnTcJXVYBDwLlYfvK7INFLwz/4pvklGQOa/ZSHdMLsR5aHLpvfgm6gTlMszV63k8NxCgFghN6B6xdftcdQPskzEYXdpqn+++40bbYv3hReA1zaOCDupjrJirYcNP3P+f6V7t1MLkQqX2pqh3TJEkorrJH/0e1WMbHUdQY6NiJcsMN56qEntdQI6GiUQVNtwOj8sH9kMMdgdjbg=="></klix-checkout>

</details>
<!-- markdownlint-disable MD033 -->

## 3. Provide integration configuration in Merchant Console

Follow registration link in Merchant Console invitation e-mail and specify new password. After registration follow [these steps](../configuration/) to configure integration.

## 4. Add widget HTML code to your web-page

Add Klix widget HTML code to your product or checkout page depending on a type of used widget (instant, checkout, payment) and your webshop specifics. More information about widget configuration can be found on [Widget](../widget/) page.

## 5. Implement callbacks end-point

[Callback](../callbacks/) end-points should be implemented in case if Klix Pay or Klix Checkout Widget is used.

## 6. Test integration

Klix provides fully functional test environment that can be used to perform end to end [integration implementation testing](../testing-integration/).

## 7. Sign agreement and request access to production environment

After integration has beed successfully tested on test environment you can proceed with creadentials request for production.
