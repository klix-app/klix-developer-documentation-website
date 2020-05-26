# Integration testing

## Environments

Klix provides a fully functional test environment that can be used to test Klix integration implementation without a need to use real bank cards or make actual transactions. Follow instructions in the [step by step guide](../step-by-step/) to receive access to test both Merchant Console and API.

| Service                     | Test environment                                                 | Production environment                                            |
|-----------------------------|------------------------------------------------------------------|-------------------------------------------------------------------|
| Merchant Console           | <sub>https://mc.stage.klix.app</sub>                             | <sub>https://mc.klix.app</sub>                                    |
| Merchant API                | <sub>https://api.stage.klix.app</sub>                            | <sub>https://api.klix.app</sub>                                   |
| Widget Javascript base path | <sub>https://klix.blob.core.windows.net/stage/widget/build/</sub>| <sub>https://klix.blob.core.windows.net/public/widget/build/</sub>|

## Test environment bank cards

Test environment is not connected to real bank card issuer networks therefore only special test cards can be used in test environment.

| Issuer     | PAN                 | CVV | Expiry date | 3D secure password |
|------------|---------------------|-----|-------------|--------------------|
| VISA       | 4314 2200 0000 0056 | 123 | 01/22       | hint               |
| MASTERCARD | 5413 3300 0000 0019 | 589 | 01/22       | hint               |

Note that any cardholder name can be used with these cards.
