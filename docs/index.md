# Overview

This documentation describes [Klix](https://klix.app) checkout/payment solution integration. Multiple integration scenarios are covered:

* Simple merchant home page that does not have an in-built shopping cart functionality. Klix Widget can be predefined to allow customers to buy a specific product. Klix Widget HTML code should be placed on a product page. No additional technical integration actions need to be performed by the merchant if orders are managed in Klix.
* Custom built or standard e-commerce platform webshop. In this case besides placing Klix Widget HTML code on a merchant check-out page payment completion callback end-point should be implemented in merchant webshop. Klix server calls this end-point for both successful and failed payments.
