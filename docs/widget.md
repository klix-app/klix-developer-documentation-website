# Klix Widget

## Klix Widget placement

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

## Widget Configuration

Klix widget is created and configured in _Merchant Console_. Generated _Widget ID_ is used to embed Web Component into Merchant HTML page.

### Specifying widget language

Widget language can be specified using attribute _language_ e.g. "lv" for Latvian, "en" for English or "ru" for Russian.

```html
<klix-checkout language="lv" widget-id="604419cc-c313-4e47-93e8-330385669bdb">
</klix-checkout>
```

### Prefilling Klix widget form data

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

## INSTANT (Fixed Price) Widget Type

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

## CHECKOUT (Dynamic Price) Widget Type

Order items should be provided either via _Widget HTML API_ (widget tag attributes) or via _Widget JS API_. Widget language is defined in the same manner as in _Fixed Price Widget_ type.

### HTML API for single order item

Tax could be defined as rate (0.XX) or percentage (number greater than 1 without ‘%’ symbol).

Tax rate default value is merchant specific (e.g. for merchants registered in LV it would be "0.21"). Currency default value is merchant specific as well (e.g. for merchants registered in EU it would be "EUR").

All HTML widgets below are equivalent for merchant registered in LV:

```html
<klix-checkout widget-id="..." amount="1.99" currency="EUR" label="Philips XR3857" tax-rate="0.21" count="1" unit="PIECE"></klix-checkout>

<klix-checkout widget-id="..." amount="1.99" currency="EUR" label="Philips XR3857" tax-rate="21"></klix-checkout>

<klix-checkout widget-id="..." amount="1.99" currency="EUR" label="Philips XR3857"></klix-checkout>

<klix-checkout widget-id="..." amount="1.99" label="Philips XR3857"></klix-checkout>
```

### HTML API for multiple order items

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

#### Implementation details

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

### JS API for single and multiple order items

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

### Order Item attributes

| Name        | Mandatory | Default value                      | Description                                          |
|-------------|-----------|------------------------------------|------------------------------------------------------|
| amount      | Yes       | n/a                                | Price per unit                                       |
| currency    | No        | Merchant specific (EUR in Latvia)  | Alphabetic code ISO 4217                             |
| label       | Yes       | n/a                                | Product or service name                              |
| count       | No        | 1                                  | Quantity                                             |
| unit        | No        | PIECE                              | {PIECE, KILOGRAM, METER, LITRE, HOUR}                |
| taxRate     | No        | Merchant specific (0.21 in Latvia) | VAT                                                  |
| orderItemId | No        | null                               | Product ID in merchant system for reference purposes |

## Javascript callbacks

Klix widget uses [Custom DOM events](https://developer.mozilla.org/en-US/docs/Web/Guide/Events/Creating_and_triggering_events) to communicate with merchant page. In order to listen specifc Klix widget event appropriate event listener should be registered first.

### Payment completed callback

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
    });
</script>
```

Following event properties can be used:

| Property                         | Type    | Description                              |
|----------------------------------|-------- |------------------------------------------|
| event.detail["orderId"]          | string  | Completed payment order identifier       |
| event.detail["paymentSucceeded"] | boolean | Indicates if payment succeeded or failed |

## Limiting accepted cards

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
