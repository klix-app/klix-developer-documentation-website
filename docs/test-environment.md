# Test environment API credentials

## Testing integration with test cards

In order to perform integration testing in recurring payment, reservation scenarios or in case production like callback data is needed to test some integration scenario following test account API credentials can be used:

| Brand ID                                                  | Secret key                                                                                                     |
|-----------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
| <sub><sup>702314b8-dd86-41fa-9a22-510fdd71fa92</sup></sub>| <sub><sup>IB-bzOdJLgJjbsaA34Qpxkg1TTIrW-iDuni6JuzbP--KgtsREHzvIvLLTH8E5T0CZcSbYM3qNmfeogpWW_RZaA== </sup></sub>|

Note that this test account has access only yo Klix card payment method on test environment and specific test cards should be used for testing:

| Issuer     | PAN                 | CVV | Expiry date | 3D secure password |
|------------|---------------------|-----|-------------|--------------------|
| VISA       | 4314 2200 0000 0056 | 123 | 01/24       | hint               |
| MASTERCARD | 5413 3300 0000 0019 | 589 | 01/24       | hint               |

Any cardholder name can be used with these cards.

Please try to make payment with both cards in case one card payments fails.

## Simplified integration testing

In order to preview Klix payment gateway functionality and test integration implementation following test account API credentials can be used:

| Brand ID                                                  | Secret key                                                                                                     |
|-----------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
| <sub><sup>702314b8-dd86-41fa-9a22-510fdd71fa92</sup></sub>| <sub><sup>No51P_Dq4jQGeha6_eQpfjAFe67u3QYHEO95jrcCux0zPfByd8x9poSa6xINQPz1hyUGKNYoxa16rnUkSUI_MA==</sup></sub> |

In case of Klix card payment method after redirecting customer to the Klix payment page (field "checkout_url" value from Purchase creation response) there's an option to choose either successful or failed payment scenario for testing purposes. In case of bank transfers and Klix Pay Later only success scenario is supported in test environment.

![Choose successful or failed payment scenario](images/testing_integration.png "Tesing integration")
