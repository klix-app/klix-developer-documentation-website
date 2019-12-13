# Klix API

## Version: 1

### Security
**token**  

|apiKey|*API Key*|
|---|---|
|Name|Authorization|
|In|header|

### /merchants/{merchantId}/orders

#### DELETE
##### Summary:

rejectMerchantOrderByExternalId

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| externalOrderId | query | externalOrderId | Yes | string |
| merchantId | path | merchantId | Yes | string (uuid) |
| rawBody | body | rawBody | Yes | string |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | OK |

### /merchants/{merchantId}/orders/{orderId}

#### GET

##### Summary

getOrder

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| X-KLIX-Api-Key | header | X-KLIX-Api-Key | Yes | string |
| merchantId | path | merchantId | Yes | string (uuid) |
| orderId | path | orderId | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK | [MerchantOrderDTO](#merchantorderdto) |
| 500 | Internal Server Error | [ErrorResponseModel](#errorresponsemodel) |

#### PUT
##### Summary:

verifyMerchantOrder

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| merchantId | path | merchantId | Yes | string (uuid) |
| orderId | path | orderId | Yes | string (uuid) |
| rawBody | body | rawBody | Yes | string |

##### Responses

| Code | Description | Schema |
| ---- | ----------- | ------ |
| 200 | OK |  |
| 500 | Internal Server Error | [ErrorResponseModel](#errorresponsemodel) |

#### DELETE
##### Summary:

rejectMerchantOrder

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| merchantId | path | merchantId | Yes | string (uuid) |
| orderId | path | orderId | Yes | string (uuid) |
| rawBody | body | rawBody | Yes | string |

##### Responses

| Code | Description |
| ---- | ----------- |
| 200 | OK |

### Models

#### Address

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| city | string |  | No |
| country | string |  | No |
| latitude | double |  | No |
| longitude | double |  | No |
| postal_code | string |  | No |
| street | string |  | No |

#### Card

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| bin | string |  | No |

#### Company

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| address | string |  | No |
| name | string |  | No |
| registration_number | string |  | No |
| vat_number | string |  | No |

#### Customer

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| email | string |  | No |
| first_name | string |  | No |
| last_name | string |  | No |
| phone_number | string |  | No |

#### MerchantOrderDTO

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| amount | number |  | No |
| company | [Company](#company) |  | No |
| currency | string |  | No |
| customer | [Customer](#customer) |  | No |
| effective_amount | number |  | No |
| id | string (uuid) |  | No |
| items | [ [OrderItem](#orderitem) ] |  | No |
| merchant_urls | [MerchantUrls](#merchanturls) |  | No |
| order_id | string |  | No |
| payment | [Payment](#payment) |  | No |
| shipping | [Shipment](#shipment) |  | No |
| status | string |  | No |
| tax_amount | number |  | No |
| total_amount | number |  | No |

#### MerchantShippingMethod

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| id | string |  | No |
| name | string |  | No |

#### MerchantUrls

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| confirmation | string |  | No |
| place_order | string |  | No |
| terms | string |  | No |
| verification | string |  | No |

#### OrderItem

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| amount | number |  | No |
| label | string |  | No |
| order_item_id | string |  | No |
| quantity | number |  | No |
| tax_amount | number |  | No |
| tax_rate | number |  | No |
| total_amount | number |  | No |
| type | string |  | No |
| unit | string |  | No |

#### Payment

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| card | [Card](#card) |  | No |
| error | [PaymentError](#paymenterror) |  | No |
| method | string |  | No |
| status | string |  | No |

#### PaymentError

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| code | string |  | No |
| message | string |  | No |

#### PickupPoint

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| comments | string |  | No |
| external_id | string |  | No |
| name | string |  | No |
| service_hours | string |  | No |

#### Shipment

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| address | [Address](#address) |  | No |
| contact_phone | string |  | No |
| date | string |  | No |
| method | [MerchantShippingMethod](#merchantshippingmethod) |  | No |
| pickup_point | [PickupPoint](#pickuppoint) |  | No |
| type | string |  | No |