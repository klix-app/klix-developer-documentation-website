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
##### Summary:

getOrder

##### Parameters

| Name | Located in | Description | Required | Schema |
| ---- | ---------- | ----------- | -------- | ---- |
| X-KLIX-Api-Key | header | X-KLIX-Api-Key | Yes | string |
| merchantId | path | merchantId | Yes | string (uuid) |
| orderId | path | orderId | Yes | string (uuid) |

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


#### MerchantOrderDTO

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| company | [Company](#company) |  | No |
| customer | [Customer](#customer) |  | No |
| id | string (uuid) |  | No |
| merchant_urls | [MerchantUrls](#merchanturls) |  | No |
| order_amount | number |  | No |
| order_lines | [ [OrderItem](#orderitem) ] |  | No |
| order_tax_amount | number |  | No |
| purchase_currency | string |  | No |
| shipment | [Shipment](#shipment) |  | No |
| status | string |  | No |

#### MerchantUrls

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| confirmation | string |  | No |
| place_order | string |  | No |
| terms | string |  | No |
| verification | string |  | No |

#### ErrorResponseModel

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| errorCode | string |  | No |
| errorId | string |  | No |
| errors | [ [ErrorModel](#errormodel) ] |  | No |
| timestamp | dateTime |  | No |

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

#### OrderItem

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| amount | number |  | No |
| name | string |  | No |
| quantity | number |  | No |
| reference | string |  | No |
| tax_rate | number |  | No |
| type | string |  | No |
| unit | string |  | No |

#### Shipment

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| address | [Address](#address) |  | No |
| contact_phone | string |  | No |
| date | string |  | No |
| method | [MerchantShippingMethod](#merchantshippingmethod) |  | No |
| pickup_point | [PickupPoint](#pickuppoint) |  | No |
| type | string |  | No |

#### ErrorModel

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| code | string |  | No |
| message | string |  | No |

#### Address

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| city | string |  | No |
| country | string |  | No |
| latitude | double |  | No |
| longitude | double |  | No |
| postal_code | string |  | No |
| street | string |  | No |

#### MerchantShippingMethod

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| id | string |  | No |
| name | string |  | No |

#### PickupPoint

| Name | Type | Description | Required |
| ---- | ---- | ----------- | -------- |
| comments | string |  | No |
| external_id | string |  | No |
| name | string |  | No |
| service_hours | string |  | No |