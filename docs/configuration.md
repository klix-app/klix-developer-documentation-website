# Integration configuration

Integration configuration is done in Klix Merchant Console. After logging in Merchant Console follow these steps to configure Klix integration.

## 1. Complete the merchant profile in Merchant Console

Fill in merchant data in "Business Profile view". Specify URL to merchant's terms & conditions. Specify [callback URLs](../callbacks/) in case advanced integration (API & callbacks based) is used.

![Fill in merchant data in Business Profile view](images/merchant_console_business_profile.png "Merchant business profile")

## 2. Download Klix public key

Head to Certificates page and copy public key contents from "Service Provider Certificate info" section. Store public key contents in file and use this key to validate the Klix callback payload.

!!! Warning "Callback payload validation"
    Merchant is obligated to perform JWS validation of Klix callback payload using Klix public key in order to ensure request payload integrity and authenticity.

![Download Klix public key](images/merchant_console_certificates.png "Klix public key")

## 3. Store API key

For each HTTP request sent to Klix header called `X-KLIX-Api-Key` should be specified. Header value can be obtained from Certificates page "Merchant Api key info" section's field "Api key".

![Add Klix API key to HTTP request headers](images/merchant_console_api_key.png "Klix API key")

## 4. Generate certificate for signing the API requests

Generate certificate and download private key file. All data modification requests sent to Klix should be signed using this private key. Klix will use merchant certificate public key to check each data modification request payload integrity and authenticity. In order to understand which merchant's certificate should be used to validate a request JWS header `kid` value should be specified. Header value can be obtained from Certificates page corresponding certificate's field "Name".

!!! Security notice
    Merchant is reponsible for storing certificate's private key securely. Merchant can generate a new certificate in Merchant Console and use this certificate to sign request payload. Note that in such case JWS header `kid` value should match new certificate's name.

![Create new merchant certificate](images/merchant_console_certificate_created.png "New merchant certificate creation")

## 5. Create a widget

Head to Widgets section to create a new widget. Klix widget is Klix form configuration representation that is identifiable by it's id. There are two types of widgets:

* Static widget
* Dynamic widget

See [Widget](../widget/) section for more detailed description of widget types amd configuration.
