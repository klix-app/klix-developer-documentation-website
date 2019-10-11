# Checkout JS Widget

## Checkout Widget placement

Checkout Widget is implemented as embeddable Web Component. In order to show that on merchant website KLIX JavaScript should be included in the page and custom HTML tag to be placed in the required location. See the example below for integration code.

```html
<head>
    <script type="module"
               src="https://klix.blob.core.windows.net/public/widget/build/klixwidget.esm.js">
    </script>
    <script nomodule 
               src="https://klix.blob.core.windows.net/public/widget/build/klixwidget.js">
    </script>
</head>
<body>
    KLIX embedded checkout:
    <klix-checkout language="lv" widget-id="604419cc-c313-4e47-93e8-330385669bdb">
    </klix-checkout>
</body>
```

Integration code could be accessed and copied from Merchant Console application.

## Widget Configuration

Widget entity with according _Widget Type_ should be created and configured in _Merchant Console_ (MC). Generated _Widget ID_ is used later for embedding Web Component into Merchant HTML page.

## INSTANT (Fixed Price) Widget Type

It is possible to generate a checkout widget for a fixed amount. In this case no validation on merchant side is required - all order items are defined at widget configuration in Merchant Console (MC). They can’t be overwritten by _Widget HTML API_ or _Widget JS API_ (specified later in the document) for security purposes. This type of Widget allows lightweight integration with merchant - no merchant back-end is required for integration.

Widget language is specified either in page _<html>_ tag or in according widget tag.

```html
<html lang="lv">
...
<checkout-widget widget-id="..."></checkout-widget>
```

OR

```html
<checkout-widget widget-id="..." language="lv"></checkout-widget>
```

## CHECKOUT (Dynamic Price) Widget Type

In case Merchant has Back-end or resources to implement integration via Merchant Back-end (order verification / confirmation _REST_ end-points) it might be more convenient to use this type of widget. One instance (created in _MC_) should be enough for all Merchant orders.

Order items should be provided either via _Widget HTML API_ (widget tag attributes) or via _Widget JS API_. Widget language is defined in the same manner as in _Fixed Price Widget_ type.

### HTML API for single order item

Tax could be defined as rate (0.XX) or percentage (number greater than 1 without ‘%’ symbol). 

Tax rate default value is merchant specific (e.g. for merchants registered in LV it would be "0.21"). Currency default value is merchant specific as well (e.g. for merchants registered in EU it would be "EUR").

All HTML widgets below are equivalent for merchant registered in LV:

```html
<checkout-widget widget-id="..." amount="1.99" currency="EUR" label="Philips XR3857" tax-rate="0.21" count="1" unit="PIECE"></checkout-widget>

<checkout-widget widget-id="..." amount="1.99" currency="EUR" label="Philips XR3857" tax-rate="21"></checkout-widget>

<checkout-widget widget-id="..." amount="1.99" currency="EUR" label="Philips XR3857"></checkout-widget>

<checkout-widget widget-id="..." amount="1.99" label="Philips XR3857"></checkout-widget>
```

### HTML API for multiple order items

```html
<checkout-widget widget-id="..." order='{"items":[{"amount":40,"currency":"EUR","count":2,"unit":"PIECE","label":"Philips XR3857","taxRate":0.21},{"amount":5,"label":"Piegāde"}]}'>
</checkout-widget>
```

OR

```html
<checkout-widget widget-id="..." order="{&quot;items&quot;:[{&quot;amount&quot;:40,&quot;currency&quot;:&quot;EUR&quot;,&quot;count&quot;:2,&quot;label&quot;:&quot;Philips XR3857&quot;,&quot;taxRate&quot;:0.21},{&quot;amount&quot;:5,&quot;label&quot;:&quot;Piegāde&quot;}]}">
</checkout-widget>
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
<checkout-widget id="my-checkout-widget" widget-id="..." lazy="true"></checkout-widget>
...
<script>
  let checkoutWidget = document.querySelector('#my-checkout-widget');
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

<table>
  <tr>
   <td>Name
   </td>
   <td>Mandatory
   </td>
   <td>Default Value
   </td>
   <td>Description
   </td>
  </tr>
  <tr>
   <td><code>amount</code>
   </td>
   <td><strong>Yes</strong>
   </td>
   <td>n/a
   </td>
   <td>Price per unit
   </td>
  </tr>
  <tr>
   <td><code>currency</code>
   </td>
   <td>No
   </td>
   <td>Merchant specific (<code>EUR</code> in Latvia)
   </td>
   <td>Alphabetic code ISO 4217
   </td>
  </tr>
  <tr>
   <td><code>label</code>
   </td>
   <td><strong>Yes</strong>
   </td>
   <td>n/a
   </td>
   <td>Product or service name
   </td>
  </tr>
  <tr>
   <td><code>count</code>
   </td>
   <td>No
   </td>
   <td><code>1</code>
   </td>
   <td>Quantity
   </td>
  </tr>
  <tr>
   <td><code>unit</code>
   </td>
   <td>No
   </td>
   <td><code>PIECE</code>
   </td>
   <td>{ PIECE, KILOGRAM, METER, LITRE, HOUR }
   </td>
  </tr>
  <tr>
   <td><code>taxRate</code>
   </td>
   <td>No
   </td>
   <td>Merchant specific (<code>0.21</code> in Latvia)
   </td>
   <td>VAT
   </td>
  </tr>
  <tr>
   <td><code>orderItemId</code>
   </td>
   <td>No
   </td>
   <td><code>null</code>
   </td>
   <td>Product ID in merchant system for reference purposes.
   </td>
  </tr>
</table>
