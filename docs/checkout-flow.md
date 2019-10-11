# Checkout Flow

Order placement could be reflected as sequence diagram with three main parties: customer, KLiX system and Merchant eCommerce system.

![alt_text](images/checkout_flow.png "Checkout flow sequence diagram")

1. **Place new order** - customer is creating new order in one of merchant sales channels (mobile, recurring payment, checkout widget). 
2. **Notify about new order** - merchant gets notified about new pending order. Merchant gets information about items being requested as well as shipment options and specified shipment address. Merchant is required to make a decision about either approving or declining the submitted order.
3. **Get pending order details** - Merchant receives a callback about new order without all order details. Merchant is required to retrieve a detailed information about order to make a decision.
4. **Validate order details and inventory** - after receiving full order data merchant should check pricing, shipment options and inventory availability. If everything is ok, merchant should approve order, if not order should be declined. At this moment funds are not yet authorized from customer payment method.
5. **Approve order** - order gets approved after inventory and pricing check as well as internal fraud rules if such exist. Items should be marked as unavailable in the internet shop at this moment.
6. **Decline order** - order gets declined due to item unavailability or price mismatch with shop inventory database. Customer sees decline message on Checkout.
7. **Order status notification** - order status is presented to the customer in Checkout widget. There is a final approval by customer for final amount.
8. **Authorize payment** - payment gets authorized by payment method specific way. This could be either 3D Secure with 2FA for payment cards or KLIX Pin or Fingerprint in case of onboarded mobile user.
9. **Payment approval** - payment approval notification is shown to the customer, so the customer knows that funds have been authorized.
10. **Confirm purchase** - KLIX send notification to merchant about funds availability and order could be dispatched. In case there is no notification in specified time frame (e.g. 10 minutes) order could be expired and reserved items released back to being available for other customers.
