# Integration testing

## Environments

Klix provides fully functional test environment that can be used to test Klix integration implementation without a need to use real bank cards or make actual transactions. Follow instructions in [quick start guide](../quick-start-guide/) to receive access to test both Merchant Console and API.

| Service           | Test environment           | Production environment     |
|-------------------|----------------------------|----------------------------|
| Merchant Conssole | https://mc.stage.klix.app  | https://mc.klix.app        |
| Merchant API      | https://api.stage.klix.app | https://api.klix.app       |

## Test environment bank cards

Test environment is not connected to real bank card issuer networks therefore only special test cards can be used in test environment.

| Issuer     | PAN                 | CVV | Expiry date | 3D secure password |
|------------|---------------------|-----|-------------|--------------------|
| VISA       | 4314 2200 0000 0056 | 123 | 01/20       | hint               |
| MASTERCARD | 5413 3300 0000 0019 | 589 | 01/20       | hint               |

Note that any cardholder name can be used with these cards.
