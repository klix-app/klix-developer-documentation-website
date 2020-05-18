# Merchant API v2

Klix provides merchant API that can be used to perform various operations with orders e.g. retrieve order by identifier, fully or partially refund an order payment transaction, etc. Note that Klix currently does not provide an API to make a payment without interacting with Klix Widget.

## Swagger documentation

Up to date Klix Merchant API [Swagger](https://swagger.io/) documentation is available [here](https://api.stage.klix.app/v2/merchants) and can be previewed using Swagger Inspector [online](https://inspector.swagger.io/builder?url=https%3A%2F%2Fapi.stage.klix.app%2Fv2%2Fmerchants).

## Authentication

### Authentication method

All requests sent to Klix Merchant API should be digitally signed using merchant certificate generated in Klix Merchant Console. Signing process is performed according to ["Draft cavage HTTP Signatures"](https://www.ietf.org/archive/id/draft-cavage-http-signatures-12.txt) specification.
Request signature is sent as an HTTP `Authorization` header value together with metadata describing signature algorithm and certificate key used.

### Signing requests without request body

The following HTTP headers should be sent for all requests without a request body: Authorization, Date.
Signed HTTP GET request example:

```sh
curl -X GET \
  https://api.stage.klix.app/v2/merchants/052eb290-a6ca-41f0-aac5-fd4ca14632e3/orders/a6f0f3c7-e3e7-4182-bc4c-bddec2a89d9a \
  -H 'Authorization: Signature keyId="ad537135-59f5-4073-8f8e-1137fc19b9d2",algorithm="rsa-sha256",headers="date (request-target)",signature="xkdqOeB0Iy4iaSQRgIWsyCuSewMzOcDISMdKO6ThFiRrvZXxdVnW1xY6WZH8E3y2qdyazSUOfMuPstVQE1ATLdUDeZFiCgRO4iYcjEviSSsXQuKT3agtad2qg8pza8w2rCmfuCjFZ87ntPKFfYqMkNTHkZB4EM/l+JEDwRXiOhRTfyVQU/xRzzq+PQ3rzSAGAPceoAmzIp6QgRVZDDAO67ktYVJ2Y9qfKGBggtQwXkZPsDy622zP8hODYLqUQlL3qsAgOL6a0RlqpOKFWJZEt4ZHfY3qBxHFUhlMyQfExZyZXMOrt4/SNNPNJwUkNbHLYXVVRuGGh2Wmc78vP464Tw=="' \
  -H 'date: Fri, 15 May 2020 10:28:40 GMT'
```

Note that HTTP header `Authorization` starts with `Signature` and thus specifies that HTTP Signatures authentication type should be used. Signature metadata description is summarized in a following table:

| Field  name          | Sample value                         | Description                                                                                 |
|----------------------|--------------------------------------|---------------------------------------------------------------------------------------------|
| keyId                | ad537135-59f5-4073-8f8e-1137fc19b9d2 | Merchant certificate name that can be found in Certificates section in Klix Merchant Console|
| algorithm            | rsa-sha256                           | Encryption algorithm used. Supported algorithms: rsa-sha256, rsa-sha384, rsa-sh512          |
| headers              | date (request-target)                | HTTP headers that are included in signature. For HTTP requests without body at least `date` and `request-target` headers should be included. Field `(request-target)` is not a HTTP header but rather a combination of HTTP method and request URI e.g. in this case `get /v2/merchants/052eb290-a6ca-41f0-aac5-fd4ca14632e3/orders/a6f0f3c7-e3e7-4182-bc4c-bddec2a89d9a`. In case additional HTTP headers are included in signature these fields should also be added to headers list |

### Signing requests with request body

The following HTTP headers should be sent for all requests with a request body: Authorization, Date, Content-Type, Digest.
Signed HTTP POST request example:

```sh
curl -X POST \
  https://api.stage.klix.app/v2/merchants/052eb290-a6ca-41f0-aac5-fd4ca14632e3/orders/a6f0f3c7-e3e7-4182-bc4c-bddec2a89d9a/refunds \
  -H 'Authorization: Signature keyId="ad537135-59f5-4073-8f8e-1137fc19b9d2",algorithm="rsa-sha256",headers="digest date content-type (request-target)",signature="xkdqOeB0Iy4iaSQRgIWsyCuSewMzOcDISMdKO6ThFiRrvZXxdVnW1xY6WZH8E3y2qdyazSUOfMuPstVQE1ATLdUDeZFiCgRO4iYcjEviSSsXQuKT3agtad2qg8pza8w2rCmfuCjFZ87ntPKFfYqMkNTHkZB4EM/l+JEDwRXiOhRTfyVQU/xRzzq+PQ3rzSAGAPceoAmzIp6QgRVZDDAO67ktYVJ2Y9qfKGBggtQwXkZPsDy622zP8hODYLqUQlL3qsAgOL6a0RlqpOKFWJZEt4ZHfY3qBxHFUhlMyQfExZyZXMOrt4/SNNPNJwUkNbHLYXVVRuGGh2Wmc78vP464Tw=="' \
  -H 'Content-Type: application/json' \
  -H 'content-length: 130' \
  -H 'date: Fri, 15 May 2020 10:28:40 GMT' \
  -H 'digest: sha-256=HoohlqpLJgeDLYagaolWSlbRC7/eMwK1O9TwgT27DzU=' \
  -d '{
    "refundRequest": {
        "amount": 5.00,
        "reason": "OTHER_REFUND",
        "note": "Internal note visible in Merchant Console"
    }
}'
```

Signature metadata description that differs from HTTP requests without request body is summarized in a following table:

| Field  name          | Sample value                                         | Description                                                                                 |
|----------------------|------------------------------------------------------|---------------------------------------------------------------------------------------------|
| headers              | digest date content-type (request-target)            | HTTP headers that are included in signature. For HTTP requests with body at least `digest`, `date`, `content-type` and `request-target` headers should be included. Field `(request-target)` is not a HTTP header but rather a combination of HTTP method and request URI e.g. in this case `post /v2/merchants/052eb290-a6ca-41f0-aac5-fd4ca14632e3/orders/a6f0f3c7-e3e7-4182-bc4c-bddec2a89d9a/refunds`. In case additional HTTP headers are included in signature these fields should also be added to headers list |

HTTP header `digest` consists of HTTP request body digest algorithm and digest value seperated by `=` sign i.e. `sha-256=base64(sha256(request body))`. Supported digest algorithms are SHA-256, SHA-384 and SHA-512. See [additional details](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Digest).

### Libraries

Here is a list of libraries that can be used to simplify merchant authentication in Klix using HTTP Signatures specification. Note that this is not an extensive list and there might be a better candidate for programming language and framework used in your project.

| Programming language | Library                                                                                     |
|----------------------|---------------------------------------------------------------------------------------------|
| Go                   | [99designs/httpsignatures-go](https://github.com/99designs/httpsignatures-go)               |
| Java                 | [tomitribe/http-signatures-java](https://github.com/tomitribe/http-signatures-java)         |
| node.js              | [joyent/node-http-signature](https://github.com/joyent/node-http-signature)                 |
| PHP                  | [99designs/http-signatures-php](https://github.com/99designs/http-signatures-php)           |
| Ruby                 | [99designs/http-signatures-ruby](https://github.com/99designs/http-signatures-ruby)         |
