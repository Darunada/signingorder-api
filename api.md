# SigningOrder.com API ((specification))

This document is an initial specification for the SigningOrder.com Public API.  This is just a description of the API and it is subject to change at any time.

## Getting Started
The API endpoint is ```((placeholder))``` and only secure TLS connections will be accepted.

### Authentication
TODO: Need More information about how the target software authenticates

### Requests
Several headers are required to be included in every request.
```
Accept: application/vnd.signingorder.api+json; version=1
Content-Type: application/json
```

In most cases, the request content should be a JSON encoded string containing the request body, as described in the specification below.

#### Ranges
List Requests will return a ```Content-Range``` header indicating the range of values returned.  Large lists may require additional requests to retrieve. If a list response has been truncated you will receive a ```206 Partial Content``` status and the ```Next-Range``` header set. To retrieve the next range, repeat the request with the ```Range``` header set to the value of the previous request’s ```Next-Range``` header.

For more information about how ranges work, please view [this reference @ Heroku](https://devcenter.heroku.com/articles/platform-api-reference#ranges)

#### Rate Limits
SigningOrder reserves the right to impose rate limiting at a future time.  If the rate limit is reached, the system will return response status ```429 Too Many Requests```

### Responses
The following status codes indicate a successful response. 

* ```200```: Request succeeded for a GET, POST, DELETE, or PATCH call that completed synchronously, or a PUT call that synchronously updated an existing resource
* ```201```: Request succeeded for a POST, or PUT call that synchronously created a new resource. It is also best practice to provide a 'Location' header pointing to the newly created resource. This is particularly useful in the POST context as the new resource will have a different URL than the original request.
* ```202```: Request accepted for a POST, PUT, DELETE, or PATCH call that will be processed asynchronously
* ```206```: Request succeeded on GET, but only a partial response returned: see above on ranges

Pay attention to the use of authentication and authorization error codes

* ```401 Unauthorized```: Request failed because user is not authenticated
* ```403 Forbidden```: Request failed because user does not have authorization to access a specific resource
* ```422 Unprocessable Entity```: Your request was understood, but contained invalid parameters
* ```429 Too Many Requests```: You have been rate-limited, retry later
* ```500 Internal Server Error```: Something went wrong on the server, check status site and/or report the issue

### Webhooks
SigningOrder can initiate HTTP POST requests to your configured callback URL when events occur with your orders within our platform.  The following events are sent.

* Signed Document Uploaded
* Order Completed
* Order Assigned to Notary
* Other status change events...
* Note Added


#### Securing Webhooks
To ensure the authenticity of event requests, SigningOrder will sign the request and post the signature along with the other webhook parameters.  To verify the webhook, users must contatenate the timestamp and token values, then encoude the resulting string with the HMAC algorithm using your API Key.  The result will match the signature provided in the request.

# The table of contents
  - <a href="#resource-additional_contact">Additional Contact</a>
    - <a href="#link-POST-additional_contact-/additional-contacts">POST /additional-contacts</a>
    - <a href="#link-DELETE-additional_contact-/additional-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}">DELETE /additional-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}</a>
    - <a href="#link-GET-additional_contact-/additional-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}">GET /additional-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}</a>
    - <a href="#link-GET-additional_contact-/additional-contacts">GET /additional-contacts</a>
    - <a href="#link-PATCH-additional_contact-/additional-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}">PATCH /additional-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}</a>
  - <a href="#resource-address">Addresses</a>
    - <a href="#link-GET-address-/addresses/{(%23%2Fdefinitions%2Faddress%2Fdefinitions%2Fidentity)}">GET /addresses/{(%23%2Fdefinitions%2Faddress%2Fdefinitions%2Fidentity)}</a>
    - <a href="#link-PATCH-address-/addresses/{(%23%2Fdefinitions%2Faddress%2Fdefinitions%2Fidentity)}">PATCH /addresses/{(%23%2Fdefinitions%2Faddress%2Fdefinitions%2Fidentity)}</a>
  - <a href="#resource-document">Document</a>
    - <a href="#link-PUT-document-/orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}/documents">PUT /orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}/documents</a>
    - <a href="#link-PATCH-document-/orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}/documents/{(%23%2Fdefinitions%2Fdocument%2Fdefinitions%2Fidentity)}">PATCH /orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}/documents/{(%23%2Fdefinitions%2Fdocument%2Fdefinitions%2Fidentity)}</a>
    - <a href="#link-DELETE-document-/orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}/documents/{(%23%2Fdefinitions%2Fdocument%2Fdefinitions%2Fidentity)}">DELETE /orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}/documents/{(%23%2Fdefinitions%2Fdocument%2Fdefinitions%2Fidentity)}</a>
    - <a href="#link-GET-document-/orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}/documents/{(%23%2Fdefinitions%2Fdocument%2Fdefinitions%2Fidentity)}">GET /orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}/documents/{(%23%2Fdefinitions%2Fdocument%2Fdefinitions%2Fidentity)}</a>
    - <a href="#link-GET-document-/orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}/documents/{(%23%2Fdefinitions%2Fdocument%2Fdefinitions%2Fidentity)}/{(%23%2Fdefinitions%2Fdocument%2Fdefinitions%2Fname)}">GET /orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}/documents/{(%23%2Fdefinitions%2Fdocument%2Fdefinitions%2Fidentity)}/{(%23%2Fdefinitions%2Fdocument%2Fdefinitions%2Fname)}</a>
    - <a href="#link-GET-document-/orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}/documents">GET /orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}/documents</a>
  - <a href="#resource-inhouse_contact">Inhouse Contact</a>
    - <a href="#link-POST-inhouse_contact-/inhouse-contacts">POST /inhouse-contacts</a>
    - <a href="#link-DELETE-inhouse_contact-/inhouse-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}">DELETE /inhouse-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}</a>
    - <a href="#link-GET-inhouse_contact-/inhouse-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}">GET /inhouse-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}</a>
    - <a href="#link-GET-inhouse_contact-/inhouse-contacts">GET /inhouse-contacts</a>
    - <a href="#link-PATCH-inhouse_contact-/inhouse-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}">PATCH /inhouse-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}</a>
  - <a href="#resource-loan_officer">Loan Officer</a>
    - <a href="#link-POST-loan_officer-/loan-officer">POST /loan-officer</a>
    - <a href="#link-DELETE-loan_officer-/loan-officer/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}">DELETE /loan-officer/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}</a>
    - <a href="#link-GET-loan_officer-/loan-officer/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}">GET /loan-officer/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}</a>
    - <a href="#link-GET-loan_officer-/loan-officer">GET /loan-officer</a>
    - <a href="#link-PATCH-loan_officer-/loan-officer/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}">PATCH /loan-officer/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}</a>
  - <a href="#resource-order">Order</a>
    - <a href="#link-POST-order-/orders">POST /orders</a>
    - <a href="#link-DELETE-order-/orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}">DELETE /orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}</a>
    - <a href="#link-GET-order-/orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}">GET /orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}</a>
    - <a href="#link-GET-order-/orders">GET /orders</a>
    - <a href="#link-PATCH-order-/orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}">PATCH /orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}</a>
  - <a href="#resource-order_contact">Order Contact</a>
    - <a href="#link-POST-order_contact-/order-contacts">POST /order-contacts</a>
    - <a href="#link-DELETE-order_contact-/order-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}">DELETE /order-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}</a>
    - <a href="#link-GET-order_contact-/order-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}">GET /order-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}</a>
    - <a href="#link-PATCH-order_contact-/order-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}">PATCH /order-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}</a>
  - <a href="#resource-zone">Zones</a>
    - <a href="#link-GET-zone-/zones/{(%23%2Fdefinitions%2Fzone%2Fdefinitions%2Fidentity)}">GET /zones/{(%23%2Fdefinitions%2Fzone%2Fdefinitions%2Fidentity)}</a>
    - <a href="#link-GET-zone-/zones">GET /zones</a>
 
## <a name="resource-additional_contact">Additional Contact</a>

Stability: `prototype`

Additional contact on order

### Attributes

| Name | Type | Description | Example |
| ------- | ------- | ------- | ------- |
| **[address_book](#resource-order_contact)** | *boolean* | in address book | `true` |
| **[created_at](#resource-order_contact)** | *date-time* | when order_contact was created | `"2015-01-01T12:00:00Z"` |
| **[id](#resource-order_contact)** | *uuid* | unique identifier of order_contact | `"01958EA1B308807443495AB1462573598756"` |
| **[name](#resource-order_contact)** | *string* | name of order_contact | `"Joe Contact"` |
| **[phone](#resource-order_contact)** | *string* | phone of order_contact<br/> **pattern:** `^\d{3}-\d{3}-\d{4}$` | `"555-555-5555"` |
| **[updated_at](#resource-order_contact)** | *date-time* | when order_contact was updated | `"2015-01-01T12:00:00Z"` |

### <a name="link-POST-additional_contact-/additional-contacts">Additional Contact Create</a>

Create a new additional contact in the address book.

```
POST /additional-contacts
```

#### Optional Parameters

| Name | Type | Description | Example |
| ------- | ------- | ------- | ------- |
| **email** | *email* | email of order_contact | `"order@contact.com"` |
| **name** | *string* | name of order_contact | `"Joe Contact"` |
| **phone** | *string* | phone of order_contact<br/> **pattern:** `^\d{3}-\d{3}-\d{4}$` | `"555-555-5555"` |


#### Curl Example

```bash
$ curl -n -X POST https://api.signingorder.com/additional-contacts \
  -d '{
  "name": "Joe Contact",
  "email": "order@contact.com",
  "phone": "555-555-5555"
}' \
  -H "Content-Type: application/json"
```


#### Response Example

```
HTTP/1.1 201 Created
```

```json
{
  "id": "01958EA1B308807443495AB1462573598756",
  "name": "Joe Contact",
  "phone": "555-555-5555",
  "address_book": true,
  "created_at": "2015-01-01T12:00:00Z",
  "updated_at": "2015-01-01T12:00:00Z"
}
```

### <a name="link-DELETE-additional_contact-/additional-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}">Additional Contact Delete</a>

Delete an existing additional contact from the address book.

```
DELETE /additional-contacts/{order_contact_id}
```


#### Curl Example

```bash
$ curl -n -X DELETE https://api.signingorder.com/additional-contacts/$ORDER_CONTACT_ID \
  -H "Content-Type: application/json"
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
{
  "id": "01958EA1B308807443495AB1462573598756",
  "name": "Joe Contact",
  "phone": "555-555-5555",
  "address_book": true,
  "created_at": "2015-01-01T12:00:00Z",
  "updated_at": "2015-01-01T12:00:00Z"
}
```

### <a name="link-GET-additional_contact-/additional-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}">Additional Contact Info</a>

Info for existing additional contact.

```
GET /additional-contacts/{order_contact_id}
```


#### Curl Example

```bash
$ curl -n https://api.signingorder.com/additional-contacts/$ORDER_CONTACT_ID
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
{
  "id": "01958EA1B308807443495AB1462573598756",
  "name": "Joe Contact",
  "phone": "555-555-5555",
  "address_book": true,
  "created_at": "2015-01-01T12:00:00Z",
  "updated_at": "2015-01-01T12:00:00Z"
}
```

### <a name="link-GET-additional_contact-/additional-contacts">Additional Contact List</a>

List existing additional contacts in the address book.

```
GET /additional-contacts
```


#### Curl Example

```bash
$ curl -n https://api.signingorder.com/additional-contacts
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
[
  {
    "id": "01958EA1B308807443495AB1462573598756",
    "name": "Joe Contact",
    "phone": "555-555-5555",
    "address_book": true,
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  }
]
```

### <a name="link-PATCH-additional_contact-/additional-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}">Additional Contact Update</a>

Update an existing additional contact.

```
PATCH /additional-contacts/{order_contact_id}
```

#### Optional Parameters

| Name | Type | Description | Example |
| ------- | ------- | ------- | ------- |
| **email** | *email* | email of order_contact | `"order@contact.com"` |
| **name** | *string* | name of order_contact | `"Joe Contact"` |
| **phone** | *string* | phone of order_contact<br/> **pattern:** `^\d{3}-\d{3}-\d{4}$` | `"555-555-5555"` |


#### Curl Example

```bash
$ curl -n -X PATCH https://api.signingorder.com/additional-contacts/$ORDER_CONTACT_ID \
  -d '{
  "name": "Joe Contact",
  "email": "order@contact.com",
  "phone": "555-555-5555"
}' \
  -H "Content-Type: application/json"
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
{
  "id": "01958EA1B308807443495AB1462573598756",
  "name": "Joe Contact",
  "phone": "555-555-5555",
  "address_book": true,
  "created_at": "2015-01-01T12:00:00Z",
  "updated_at": "2015-01-01T12:00:00Z"
}
```


## <a name="resource-address">Addresses</a>

Stability: `prototype`

A Street Address

### Attributes

| Name | Type | Description | Example |
| ------- | ------- | ------- | ------- |
| **address1** | *string* | line 1 of the address | `"56 W 9000 S"` |
| **address2** | *string* | line 2 of the address | `""` |
| **city** | *string* | city name | `"Sandy"` |
| **company** | *string* | company name of the address | `"Starbucks"` |
| **created_at** | *date-time* | when address was created | `"2015-01-01T12:00:00Z"` |
| **id** | *uuid* | unique identifier of address | `"00000021A1442D8439B95C61432235892314"` |
| **type** | *string* | address type<br/> **one of:**`"signing"` or `"property"` | `"signing"` |
| **updated_at** | *date-time* | when address was updated | `"2015-01-01T12:00:00Z"` |
| **zipcode** | *string* | zipcode | `"84070"` |
| **[zone:code](#resource-zone)** | *string* | 2 character code of zone | `"UT"` |
| **[zone:id](#resource-zone)** | *uuid* | unique identifier of zone | `"000CD01950BF7E14B11B7B50139023144915"` |
| **[zone:name](#resource-zone)** | *string* | name of zone | `"Utah"` |
| **[zone:timezone](#resource-zone)** | *string* | IANA format timezone identifier | `"America/Denver"` |

### <a name="link-GET-address-/addresses/{(%23%2Fdefinitions%2Faddress%2Fdefinitions%2Fidentity)}">Addresses Info</a>

Info for existing address.

```
GET /addresses/{address_id}
```


#### Curl Example

```bash
$ curl -n https://api.signingorder.com/addresses/$ADDRESS_ID
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
{
  "id": "00000021A1442D8439B95C61432235892314",
  "company": "Starbucks",
  "address1": "56 W 9000 S",
  "address2": "",
  "city": "Sandy",
  "zone": {
    "id": "000CD01950BF7E14B11B7B50139023144915",
    "name": "Utah",
    "code": "UT",
    "timezone": "America/Denver"
  },
  "zipcode": "84070",
  "type": "signing",
  "created_at": "2015-01-01T12:00:00Z",
  "updated_at": "2015-01-01T12:00:00Z"
}
```

### <a name="link-PATCH-address-/addresses/{(%23%2Fdefinitions%2Faddress%2Fdefinitions%2Fidentity)}">Addresses Update</a>

Update an existing address.

```
PATCH /addresses/{address_id}
```

#### Optional Parameters

| Name | Type | Description | Example |
| ------- | ------- | ------- | ------- |
| **address1** | *string* | line 1 of the address | `"56 W 9000 S"` |
| **address2** | *string* | line 2 of the address | `""` |
| **city** | *string* | city name | `"Sandy"` |
| **company** | *string* | company name of the address | `"Starbucks"` |
| **zipcode** | *string* | zipcode | `"84070"` |
| **zone** | *string* | unique identifier, name of zone or 2 character code of zone | `"000CD01950BF7E14B11B7B50139023144915"` or `"Utah"` or `"UT"` |


#### Curl Example

```bash
$ curl -n -X PATCH https://api.signingorder.com/addresses/$ADDRESS_ID \
  -d '{
  "company": "Starbucks",
  "address1": "56 W 9000 S",
  "address2": "",
  "city": "Sandy",
  "zone": "000CD01950BF7E14B11B7B50139023144915",
  "zipcode": "84070"
}' \
  -H "Content-Type: application/json"
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
{
  "id": "00000021A1442D8439B95C61432235892314",
  "company": "Starbucks",
  "address1": "56 W 9000 S",
  "address2": "",
  "city": "Sandy",
  "zone": {
    "id": "000CD01950BF7E14B11B7B50139023144915",
    "name": "Utah",
    "code": "UT",
    "timezone": "America/Denver"
  },
  "zipcode": "84070",
  "type": "signing",
  "created_at": "2015-01-01T12:00:00Z",
  "updated_at": "2015-01-01T12:00:00Z"
}
```


## <a name="resource-document">Document</a>

Stability: `prototype`

Documents related to the order

### Attributes

| Name | Type | Description | Example |
| ------- | ------- | ------- | ------- |
| **created_at** | *date-time* | when document was created | `"2015-01-01T12:00:00Z"` |
| **description** | *string* | description to go along with document | `"Signing Package"` |
| **download_url** | *file* | url to fetch the file | `"https://api.signingorder.com/orders/{order-id}/documents/{document-id}/filename.pdf"` |
| **downloaded** | *boolean* | if the notary has downloaded the document | `true` |
| **id** | *uuid* | unique identifier of document | `"01234567-89ab-cdef-0123-456789abcdef"` |
| **name** | *string* | name of document | `"filename.pdf"` |
| **updated_at** | *date-time* | when document was updated | `"2015-01-01T12:00:00Z"` |

### <a name="link-PUT-document-/orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}/documents">Document Upload</a>

Create a new document.  The document should be sent using ```Content-Type: multipart/form-data;``` in a PUT request.  The resulting document object will be returned.

```
PUT /orders/{order_id}/documents
```


#### Curl Example

```bash
$ curl -n -X PUT https://api.signingorder.com/orders/$ORDER_ID/documents \
  -H "Content-Type: application/json"
```


#### Response Example

```
HTTP/1.1 201 Created
```

```json
{
  "created_at": "2015-01-01T12:00:00Z",
  "id": "01234567-89ab-cdef-0123-456789abcdef",
  "name": "filename.pdf",
  "description": "Signing Package",
  "downloaded": true,
  "download_url": "https://api.signingorder.com/orders/{order-id}/documents/{document-id}/filename.pdf",
  "updated_at": "2015-01-01T12:00:00Z"
}
```

### <a name="link-PATCH-document-/orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}/documents/{(%23%2Fdefinitions%2Fdocument%2Fdefinitions%2Fidentity)}">Document Update</a>

Update a document's meta data.

```
PATCH /orders/{order_id}/documents/{document_id}
```

#### Optional Parameters

| Name | Type | Description | Example |
| ------- | ------- | ------- | ------- |
| **description** | *string* | description to go along with document | `"Signing Package"` |
| **name** | *string* | name of document | `"filename.pdf"` |


#### Curl Example

```bash
$ curl -n -X PATCH https://api.signingorder.com/orders/$ORDER_ID/documents/$DOCUMENT_ID \
  -d '{
  "name": "filename.pdf",
  "description": "Signing Package"
}' \
  -H "Content-Type: application/json"
```


#### Response Example

```
HTTP/1.1 201 Created
```

```json
{
  "created_at": "2015-01-01T12:00:00Z",
  "id": "01234567-89ab-cdef-0123-456789abcdef",
  "name": "filename.pdf",
  "description": "Signing Package",
  "downloaded": true,
  "download_url": "https://api.signingorder.com/orders/{order-id}/documents/{document-id}/filename.pdf",
  "updated_at": "2015-01-01T12:00:00Z"
}
```

### <a name="link-DELETE-document-/orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}/documents/{(%23%2Fdefinitions%2Fdocument%2Fdefinitions%2Fidentity)}">Document Delete</a>

Delete an existing document.

```
DELETE /orders/{order_id}/documents/{document_id}
```


#### Curl Example

```bash
$ curl -n -X DELETE https://api.signingorder.com/orders/$ORDER_ID/documents/$DOCUMENT_ID \
  -H "Content-Type: application/json"
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
{
  "created_at": "2015-01-01T12:00:00Z",
  "id": "01234567-89ab-cdef-0123-456789abcdef",
  "name": "filename.pdf",
  "description": "Signing Package",
  "downloaded": true,
  "download_url": "https://api.signingorder.com/orders/{order-id}/documents/{document-id}/filename.pdf",
  "updated_at": "2015-01-01T12:00:00Z"
}
```

### <a name="link-GET-document-/orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}/documents/{(%23%2Fdefinitions%2Fdocument%2Fdefinitions%2Fidentity)}">Document Info</a>

Info for existing document.

```
GET /orders/{order_id}/documents/{document_id}
```


#### Curl Example

```bash
$ curl -n https://api.signingorder.com/orders/$ORDER_ID/documents/$DOCUMENT_ID
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
{
  "created_at": "2015-01-01T12:00:00Z",
  "id": "01234567-89ab-cdef-0123-456789abcdef",
  "name": "filename.pdf",
  "description": "Signing Package",
  "downloaded": true,
  "download_url": "https://api.signingorder.com/orders/{order-id}/documents/{document-id}/filename.pdf",
  "updated_at": "2015-01-01T12:00:00Z"
}
```

### <a name="link-GET-document-/orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}/documents/{(%23%2Fdefinitions%2Fdocument%2Fdefinitions%2Fidentity)}/{(%23%2Fdefinitions%2Fdocument%2Fdefinitions%2Fname)}">Document Download</a>

Download existing document.  This is a direct link to the file which you can then fetch yourself using curl, etc.

```
GET /orders/{order_id}/documents/{document_id}/{document_name}
```


#### Curl Example

```bash
$ curl -n https://api.signingorder.com/orders/$ORDER_ID/documents/$DOCUMENT_ID/$DOCUMENT_NAME
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
{
  "created_at": "2015-01-01T12:00:00Z",
  "id": "01234567-89ab-cdef-0123-456789abcdef",
  "name": "filename.pdf",
  "description": "Signing Package",
  "downloaded": true,
  "download_url": "https://api.signingorder.com/orders/{order-id}/documents/{document-id}/filename.pdf",
  "updated_at": "2015-01-01T12:00:00Z"
}
```

### <a name="link-GET-document-/orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}/documents">Document List</a>

List existing documents.

```
GET /orders/{order_id}/documents
```


#### Curl Example

```bash
$ curl -n https://api.signingorder.com/orders/$ORDER_ID/documents
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
[
  {
    "created_at": "2015-01-01T12:00:00Z",
    "id": "01234567-89ab-cdef-0123-456789abcdef",
    "name": "filename.pdf",
    "description": "Signing Package",
    "downloaded": true,
    "download_url": "https://api.signingorder.com/orders/{order-id}/documents/{document-id}/filename.pdf",
    "updated_at": "2015-01-01T12:00:00Z"
  }
]
```


## <a name="resource-inhouse_contact">Inhouse Contact</a>

Stability: `prototype`

Inhouse Contact on order

### Attributes

| Name | Type | Description | Example |
| ------- | ------- | ------- | ------- |
| **[address_book](#resource-order_contact)** | *boolean* | in address book | `true` |
| **[created_at](#resource-order_contact)** | *date-time* | when order_contact was created | `"2015-01-01T12:00:00Z"` |
| **[id](#resource-order_contact)** | *uuid* | unique identifier of order_contact | `"01958EA1B308807443495AB1462573598756"` |
| **[name](#resource-order_contact)** | *string* | name of order_contact | `"Joe Contact"` |
| **[phone](#resource-order_contact)** | *string* | phone of order_contact<br/> **pattern:** `^\d{3}-\d{3}-\d{4}$` | `"555-555-5555"` |
| **[updated_at](#resource-order_contact)** | *date-time* | when order_contact was updated | `"2015-01-01T12:00:00Z"` |

### <a name="link-POST-inhouse_contact-/inhouse-contacts">Inhouse Contact Create</a>

Create a new inhouse contact in the address book.

```
POST /inhouse-contacts
```

#### Optional Parameters

| Name | Type | Description | Example |
| ------- | ------- | ------- | ------- |
| **email** | *email* | email of order_contact | `"order@contact.com"` |
| **name** | *string* | name of order_contact | `"Joe Contact"` |
| **phone** | *string* | phone of order_contact<br/> **pattern:** `^\d{3}-\d{3}-\d{4}$` | `"555-555-5555"` |


#### Curl Example

```bash
$ curl -n -X POST https://api.signingorder.com/inhouse-contacts \
  -d '{
  "name": "Joe Contact",
  "email": "order@contact.com",
  "phone": "555-555-5555"
}' \
  -H "Content-Type: application/json"
```


#### Response Example

```
HTTP/1.1 201 Created
```

```json
{
  "id": "01958EA1B308807443495AB1462573598756",
  "name": "Joe Contact",
  "phone": "555-555-5555",
  "address_book": true,
  "created_at": "2015-01-01T12:00:00Z",
  "updated_at": "2015-01-01T12:00:00Z"
}
```

### <a name="link-DELETE-inhouse_contact-/inhouse-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}">Inhouse Contact Delete</a>

Delete an existing inhouse contact from the address book.

```
DELETE /inhouse-contacts/{order_contact_id}
```


#### Curl Example

```bash
$ curl -n -X DELETE https://api.signingorder.com/inhouse-contacts/$ORDER_CONTACT_ID \
  -H "Content-Type: application/json"
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
{
  "id": "01958EA1B308807443495AB1462573598756",
  "name": "Joe Contact",
  "phone": "555-555-5555",
  "address_book": true,
  "created_at": "2015-01-01T12:00:00Z",
  "updated_at": "2015-01-01T12:00:00Z"
}
```

### <a name="link-GET-inhouse_contact-/inhouse-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}">Inhouse Contact Info</a>

Info for existing inhouse contact.

```
GET /inhouse-contacts/{order_contact_id}
```


#### Curl Example

```bash
$ curl -n https://api.signingorder.com/inhouse-contacts/$ORDER_CONTACT_ID
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
{
  "id": "01958EA1B308807443495AB1462573598756",
  "name": "Joe Contact",
  "phone": "555-555-5555",
  "address_book": true,
  "created_at": "2015-01-01T12:00:00Z",
  "updated_at": "2015-01-01T12:00:00Z"
}
```

### <a name="link-GET-inhouse_contact-/inhouse-contacts">Inhouse Contact List</a>

List existing inhouse contacts in the address book.

```
GET /inhouse-contacts
```


#### Curl Example

```bash
$ curl -n https://api.signingorder.com/inhouse-contacts
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
[
  {
    "id": "01958EA1B308807443495AB1462573598756",
    "name": "Joe Contact",
    "phone": "555-555-5555",
    "address_book": true,
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  }
]
```

### <a name="link-PATCH-inhouse_contact-/inhouse-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}">Inhouse Contact Update</a>

Update an existing inhouse contact.

```
PATCH /inhouse-contacts/{order_contact_id}
```

#### Optional Parameters

| Name | Type | Description | Example |
| ------- | ------- | ------- | ------- |
| **email** | *email* | email of order_contact | `"order@contact.com"` |
| **name** | *string* | name of order_contact | `"Joe Contact"` |
| **phone** | *string* | phone of order_contact<br/> **pattern:** `^\d{3}-\d{3}-\d{4}$` | `"555-555-5555"` |


#### Curl Example

```bash
$ curl -n -X PATCH https://api.signingorder.com/inhouse-contacts/$ORDER_CONTACT_ID \
  -d '{
  "name": "Joe Contact",
  "email": "order@contact.com",
  "phone": "555-555-5555"
}' \
  -H "Content-Type: application/json"
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
{
  "id": "01958EA1B308807443495AB1462573598756",
  "name": "Joe Contact",
  "phone": "555-555-5555",
  "address_book": true,
  "created_at": "2015-01-01T12:00:00Z",
  "updated_at": "2015-01-01T12:00:00Z"
}
```


## <a name="resource-loan_officer">Loan Officer</a>

Stability: `prototype`

Loan Officer contact on order

### Attributes

| Name | Type | Description | Example |
| ------- | ------- | ------- | ------- |
| **[address_book](#resource-order_contact)** | *boolean* | in address book | `true` |
| **[created_at](#resource-order_contact)** | *date-time* | when order_contact was created | `"2015-01-01T12:00:00Z"` |
| **[id](#resource-order_contact)** | *uuid* | unique identifier of order_contact | `"01958EA1B308807443495AB1462573598756"` |
| **[name](#resource-order_contact)** | *string* | name of order_contact | `"Joe Contact"` |
| **[phone](#resource-order_contact)** | *string* | phone of order_contact<br/> **pattern:** `^\d{3}-\d{3}-\d{4}$` | `"555-555-5555"` |
| **[private](#resource-order_contact)** | *boolean* | show to notaries | `false` |
| **[updated_at](#resource-order_contact)** | *date-time* | when order_contact was updated | `"2015-01-01T12:00:00Z"` |

### <a name="link-POST-loan_officer-/loan-officer">Loan Officer Create</a>

Create a new loan officer contact in the address book.

```
POST /loan-officer
```

#### Optional Parameters

| Name | Type | Description | Example |
| ------- | ------- | ------- | ------- |
| **email** | *email* | email of order_contact | `"order@contact.com"` |
| **name** | *string* | name of order_contact | `"Joe Contact"` |
| **phone** | *string* | phone of order_contact<br/> **pattern:** `^\d{3}-\d{3}-\d{4}$` | `"555-555-5555"` |
| **private** | *boolean* | show to notaries | `false` |


#### Curl Example

```bash
$ curl -n -X POST https://api.signingorder.com/loan-officer \
  -d '{
  "name": "Joe Contact",
  "email": "order@contact.com",
  "phone": "555-555-5555",
  "private": false
}' \
  -H "Content-Type: application/json"
```


#### Response Example

```
HTTP/1.1 201 Created
```

```json
{
  "id": "01958EA1B308807443495AB1462573598756",
  "name": "Joe Contact",
  "phone": "555-555-5555",
  "address_book": true,
  "private": false,
  "created_at": "2015-01-01T12:00:00Z",
  "updated_at": "2015-01-01T12:00:00Z"
}
```

### <a name="link-DELETE-loan_officer-/loan-officer/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}">Loan Officer Delete</a>

Delete an existing loan officer contact from the address book.

```
DELETE /loan-officer/{order_contact_id}
```


#### Curl Example

```bash
$ curl -n -X DELETE https://api.signingorder.com/loan-officer/$ORDER_CONTACT_ID \
  -H "Content-Type: application/json"
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
{
  "id": "01958EA1B308807443495AB1462573598756",
  "name": "Joe Contact",
  "phone": "555-555-5555",
  "address_book": true,
  "private": false,
  "created_at": "2015-01-01T12:00:00Z",
  "updated_at": "2015-01-01T12:00:00Z"
}
```

### <a name="link-GET-loan_officer-/loan-officer/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}">Loan Officer Info</a>

Info for existing loan officer contact.

```
GET /loan-officer/{order_contact_id}
```


#### Curl Example

```bash
$ curl -n https://api.signingorder.com/loan-officer/$ORDER_CONTACT_ID
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
{
  "id": "01958EA1B308807443495AB1462573598756",
  "name": "Joe Contact",
  "phone": "555-555-5555",
  "address_book": true,
  "private": false,
  "created_at": "2015-01-01T12:00:00Z",
  "updated_at": "2015-01-01T12:00:00Z"
}
```

### <a name="link-GET-loan_officer-/loan-officer">Loan Officer List</a>

List existing loan officer contacts in the address book.

```
GET /loan-officer
```


#### Curl Example

```bash
$ curl -n https://api.signingorder.com/loan-officer
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
[
  {
    "id": "01958EA1B308807443495AB1462573598756",
    "name": "Joe Contact",
    "phone": "555-555-5555",
    "address_book": true,
    "private": false,
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  }
]
```

### <a name="link-PATCH-loan_officer-/loan-officer/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}">Loan Officer Update</a>

Update an existing loan officer contact.

```
PATCH /loan-officer/{order_contact_id}
```

#### Optional Parameters

| Name | Type | Description | Example |
| ------- | ------- | ------- | ------- |
| **email** | *email* | email of order_contact | `"order@contact.com"` |
| **name** | *string* | name of order_contact | `"Joe Contact"` |
| **phone** | *boolean* | show to notaries | `false` |


#### Curl Example

```bash
$ curl -n -X PATCH https://api.signingorder.com/loan-officer/$ORDER_CONTACT_ID \
  -d '{
  "name": "Joe Contact",
  "email": "order@contact.com",
  "phone": false
}' \
  -H "Content-Type: application/json"
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
{
  "id": "01958EA1B308807443495AB1462573598756",
  "name": "Joe Contact",
  "phone": "555-555-5555",
  "address_book": true,
  "private": false,
  "created_at": "2015-01-01T12:00:00Z",
  "updated_at": "2015-01-01T12:00:00Z"
}
```


## <a name="resource-order">Order</a>

Stability: `prototype`

FIXME

### Attributes

| Name | Type | Description | Example |
| ------- | ------- | ------- | ------- |
| **[additional_contact:address_book](#resource-order_contact)** | *boolean* | in address book | `true` |
| **[additional_contact:created_at](#resource-order_contact)** | *date-time* | when order_contact was created | `"2015-01-01T12:00:00Z"` |
| **[additional_contact:id](#resource-order_contact)** | *uuid* | unique identifier of order_contact | `"01958EA1B308807443495AB1462573598756"` |
| **[additional_contact:name](#resource-order_contact)** | *string* | name of order_contact | `"Joe Contact"` |
| **[additional_contact:phone](#resource-order_contact)** | *string* | phone of order_contact<br/> **pattern:** `^\d{3}-\d{3}-\d{4}$` | `"555-555-5555"` |
| **[additional_contact:updated_at](#resource-order_contact)** | *date-time* | when order_contact was updated | `"2015-01-01T12:00:00Z"` |
| **closing_date** | *string* | closing_date | `"example"` |
| **closing_time** | *nullable string* | closing_time | `null` |
| **closing_time_mode** | *string* | closing time mode<br/> **one of:**`"time"` or `"tbd"` or `"asap"` | `"time"` |
| **closing_timezone** | *nullable string* | closing_timezone | `null` |
| **contact_altphone1** | *string* | alternate phone of signer<br/> **pattern:** `^\d{3}-\d{3}-\d{4}$` | `"555-555-5555"` |
| **contact_altphone1_type** | *string* | alternate phone type | `"example"` |
| **contact_altphone2** | *string* | second alterate for signer<br/> **pattern:** `^\d{3}-\d{3}-\d{4}$` | `"555-555-5555"` |
| **contact_altphone2_type** | *string* | alternate phone2 type | `"example"` |
| **contact_info** | *string* | special instructions | `"Use Blue ink only"` |
| **contact_phone** | *string* | primary phone of signer<br/> **pattern:** `^\d{3}-\d{3}-\d{4}$` | `"555-555-5555"` |
| **contact_phone_type** | *string* | contact phone type | `"example"` |
| **cosigner_first_name** | *string* | cosigner first name | `"example"` |
| **cosigner_last_name** | *string* | cosigner last name | `"example"` |
| **cosigner_spouse_first_name** | *string* | cosigner first name | `"example"` |
| **cosigner_spouse_last_name** | *string* | cosigner last name | `"example"` |
| **created_at** | *date-time* | when order was created | `"2015-01-01T12:00:00Z"` |
| **document_url** | *string* | requires faxbacks? | `"https://api.signingorder.com/orders/{(%2Fschemata%2Forder%23%2Fdefinitions%2Fidentity)}/documents"` |
| **escrow_number** | *string* | escrow number | `"example"` |
| **faxbacks** | *boolean* | requires faxbacks? | `true` |
| **id** | *uuid* | unique identifier of order | `"01958EA1B308807443495AB1462573598756"` |
| **[inhouse_contact:address_book](#resource-order_contact)** | *boolean* | in address book | `true` |
| **[inhouse_contact:created_at](#resource-order_contact)** | *date-time* | when order_contact was created | `"2015-01-01T12:00:00Z"` |
| **[inhouse_contact:id](#resource-order_contact)** | *uuid* | unique identifier of order_contact | `"01958EA1B308807443495AB1462573598756"` |
| **[inhouse_contact:name](#resource-order_contact)** | *string* | name of order_contact | `"Joe Contact"` |
| **[inhouse_contact:phone](#resource-order_contact)** | *string* | phone of order_contact<br/> **pattern:** `^\d{3}-\d{3}-\d{4}$` | `"555-555-5555"` |
| **[inhouse_contact:updated_at](#resource-order_contact)** | *date-time* | when order_contact was updated | `"2015-01-01T12:00:00Z"` |
| **[loan_officer:address_book](#resource-order_contact)** | *boolean* | in address book | `true` |
| **[loan_officer:created_at](#resource-order_contact)** | *date-time* | when order_contact was created | `"2015-01-01T12:00:00Z"` |
| **[loan_officer:id](#resource-order_contact)** | *uuid* | unique identifier of order_contact | `"01958EA1B308807443495AB1462573598756"` |
| **[loan_officer:name](#resource-order_contact)** | *string* | name of order_contact | `"Joe Contact"` |
| **[loan_officer:phone](#resource-order_contact)** | *string* | phone of order_contact<br/> **pattern:** `^\d{3}-\d{3}-\d{4}$` | `"555-555-5555"` |
| **[loan_officer:private](#resource-order_contact)** | *boolean* | show to notaries | `false` |
| **[loan_officer:updated_at](#resource-order_contact)** | *date-time* | when order_contact was updated | `"2015-01-01T12:00:00Z"` |
| **[order_contact:address_book](#resource-order_contact)** | *boolean* | in address book | `true` |
| **[order_contact:altphone](#resource-order_contact)** | *string* | alternate phone of order_contact<br/> **pattern:** `^\d{3}-\d{3}-\d{4}$` | `"444-444-4444"` |
| **[order_contact:created_at](#resource-order_contact)** | *date-time* | when order_contact was created | `"2015-01-01T12:00:00Z"` |
| **[order_contact:id](#resource-order_contact)** | *uuid* | unique identifier of order_contact | `"01958EA1B308807443495AB1462573598756"` |
| **[order_contact:name](#resource-order_contact)** | *string* | name of order_contact | `"Joe Contact"` |
| **[order_contact:phone](#resource-order_contact)** | *string* | phone of order_contact<br/> **pattern:** `^\d{3}-\d{3}-\d{4}$` | `"555-555-5555"` |
| **[order_contact:updated_at](#resource-order_contact)** | *date-time* | when order_contact was updated | `"2015-01-01T12:00:00Z"` |
| **[property_address:address1](#resource-address)** | *string* | line 1 of the address | `"56 W 9000 S"` |
| **[property_address:address2](#resource-address)** | *string* | line 2 of the address | `""` |
| **[property_address:city](#resource-address)** | *string* | city name | `"Sandy"` |
| **[property_address:company](#resource-address)** | *string* | company name of the address | `"Starbucks"` |
| **[property_address:created_at](#resource-address)** | *date-time* | when address was created | `"2015-01-01T12:00:00Z"` |
| **[property_address:id](#resource-address)** | *uuid* | unique identifier of address | `"00000021A1442D8439B95C61432235892314"` |
| **[property_address:type](#resource-address)** | *string* | address type<br/> **one of:**`"signing"` or `"property"` | `"signing"` |
| **[property_address:updated_at](#resource-address)** | *date-time* | when address was updated | `"2015-01-01T12:00:00Z"` |
| **[property_address:zipcode](#resource-address)** | *string* | zipcode | `"84070"` |
| **[property_address:zone:code](#resource-zone)** | *string* | 2 character code of zone | `"UT"` |
| **[property_address:zone:id](#resource-zone)** | *uuid* | unique identifier | `"000CD01950BF7E14B11B7B50139023144915"` |
| **[property_address:zone:name](#resource-zone)** | *string* | name of zone | `"Utah"` |
| **[property_address:zone:timezone](#resource-zone)** | *string* | IANA format timezone identifier | `"America/Denver"` |
| **return_instructions** | *string* | document return instructions | `"shipping label provided"` |
| **signer_email** | *email* | signer's email | `"signers@email.com"` |
| **signer_first_name** | *string* | signer first name | `"example"` |
| **signer_last_name** | *string* | signer last name | `"example"` |
| **signer_spouse_first_name** | *string* | signer spouse first name | `"example"` |
| **signer_spouse_last_name** | *string* | signer spuse last name | `"example"` |
| **[signing_address:address1](#resource-address)** | *string* | line 1 of the address | `"56 W 9000 S"` |
| **[signing_address:address2](#resource-address)** | *string* | line 2 of the address | `""` |
| **[signing_address:city](#resource-address)** | *string* | city name | `"Sandy"` |
| **[signing_address:company](#resource-address)** | *string* | company name of the address | `"Starbucks"` |
| **[signing_address:created_at](#resource-address)** | *date-time* | when address was created | `"2015-01-01T12:00:00Z"` |
| **[signing_address:id](#resource-address)** | *uuid* | unique identifier of address | `"00000021A1442D8439B95C61432235892314"` |
| **[signing_address:type](#resource-address)** | *string* | address type<br/> **one of:**`"signing"` or `"property"` | `"signing"` |
| **[signing_address:updated_at](#resource-address)** | *date-time* | when address was updated | `"2015-01-01T12:00:00Z"` |
| **[signing_address:zipcode](#resource-address)** | *string* | zipcode | `"84070"` |
| **[signing_address:zone:code](#resource-zone)** | *string* | 2 character code of zone | `"UT"` |
| **[signing_address:zone:id](#resource-zone)** | *uuid* | unique identifier | `"000CD01950BF7E14B11B7B50139023144915"` |
| **[signing_address:zone:name](#resource-zone)** | *string* | name of zone | `"Utah"` |
| **[signing_address:zone:timezone](#resource-zone)** | *string* | IANA format timezone identifier | `"America/Denver"` |
| **signing_type** | *integer* | signing type | `42` |
| **special_instructions** | *string* | special instructions | `"Use Blue ink only"` |
| **status** | *string* | order status | `"ASSIGNED"` |
| **updated_at** | *date-time* | when order was updated | `"2015-01-01T12:00:00Z"` |

### <a name="link-POST-order-/orders">Order Create</a>

Create a new order.

```
POST /orders
```


#### Curl Example

```bash
$ curl -n -X POST https://api.signingorder.com/orders \
  -d '{
}' \
  -H "Content-Type: application/json"
```


#### Response Example

```
HTTP/1.1 201 Created
```

```json
{
  "id": "01958EA1B308807443495AB1462573598756",
  "order_contact": {
    "id": "01958EA1B308807443495AB1462573598756",
    "name": "Joe Contact",
    "phone": "555-555-5555",
    "altphone": "444-444-4444",
    "address_book": true,
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  },
  "additional_contact": {
    "id": "01958EA1B308807443495AB1462573598756",
    "name": "Joe Contact",
    "phone": "555-555-5555",
    "address_book": true,
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  },
  "inhouse_contact": {
    "id": "01958EA1B308807443495AB1462573598756",
    "name": "Joe Contact",
    "phone": "555-555-5555",
    "address_book": true,
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  },
  "loan_officer": {
    "id": "01958EA1B308807443495AB1462573598756",
    "name": "Joe Contact",
    "phone": "555-555-5555",
    "address_book": true,
    "private": false,
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  },
  "property_address": {
    "id": "00000021A1442D8439B95C61432235892314",
    "company": "Starbucks",
    "address1": "56 W 9000 S",
    "address2": "",
    "city": "Sandy",
    "zone": {
      "id": "000CD01950BF7E14B11B7B50139023144915",
      "name": "Utah",
      "code": "UT",
      "timezone": "America/Denver"
    },
    "zipcode": "84070",
    "type": "signing",
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  },
  "signing_address": {
    "id": "00000021A1442D8439B95C61432235892314",
    "company": "Starbucks",
    "address1": "56 W 9000 S",
    "address2": "",
    "city": "Sandy",
    "zone": {
      "id": "000CD01950BF7E14B11B7B50139023144915",
      "name": "Utah",
      "code": "UT",
      "timezone": "America/Denver"
    },
    "zipcode": "84070",
    "type": "signing",
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  },
  "escrow_number": "example",
  "closing_date": "example",
  "closing_time": null,
  "closing_timezone": null,
  "closing_time_mode": "time",
  "signing_type": 42,
  "signer_first_name": "example",
  "signer_last_name": "example",
  "signer_spouse_first_name": "example",
  "signer_spouse_last_name": "example",
  "cosigner_first_name": "example",
  "cosigner_last_name": "example",
  "cosigner_spouse_first_name": "example",
  "cosigner_spouse_last_name": "example",
  "contact_phone": "555-555-5555",
  "contact_phone_type": "example",
  "contact_altphone1": "555-555-5555",
  "contact_altphone1_type": "example",
  "contact_altphone2": "555-555-5555",
  "contact_altphone2_type": "example",
  "signer_email": "signers@email.com",
  "status": "ASSIGNED",
  "special_instructions": "Use Blue ink only",
  "contact_info": "Use Blue ink only",
  "return_instructions": "shipping label provided",
  "faxbacks": true,
  "document_url": "https://api.signingorder.com/orders/{(%2Fschemata%2Forder%23%2Fdefinitions%2Fidentity)}/documents",
  "created_at": "2015-01-01T12:00:00Z",
  "updated_at": "2015-01-01T12:00:00Z"
}
```

### <a name="link-DELETE-order-/orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}">Order Delete</a>

Delete an existing order.

```
DELETE /orders/{order_id}
```


#### Curl Example

```bash
$ curl -n -X DELETE https://api.signingorder.com/orders/$ORDER_ID \
  -H "Content-Type: application/json"
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
{
  "id": "01958EA1B308807443495AB1462573598756",
  "order_contact": {
    "id": "01958EA1B308807443495AB1462573598756",
    "name": "Joe Contact",
    "phone": "555-555-5555",
    "altphone": "444-444-4444",
    "address_book": true,
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  },
  "additional_contact": {
    "id": "01958EA1B308807443495AB1462573598756",
    "name": "Joe Contact",
    "phone": "555-555-5555",
    "address_book": true,
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  },
  "inhouse_contact": {
    "id": "01958EA1B308807443495AB1462573598756",
    "name": "Joe Contact",
    "phone": "555-555-5555",
    "address_book": true,
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  },
  "loan_officer": {
    "id": "01958EA1B308807443495AB1462573598756",
    "name": "Joe Contact",
    "phone": "555-555-5555",
    "address_book": true,
    "private": false,
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  },
  "property_address": {
    "id": "00000021A1442D8439B95C61432235892314",
    "company": "Starbucks",
    "address1": "56 W 9000 S",
    "address2": "",
    "city": "Sandy",
    "zone": {
      "id": "000CD01950BF7E14B11B7B50139023144915",
      "name": "Utah",
      "code": "UT",
      "timezone": "America/Denver"
    },
    "zipcode": "84070",
    "type": "signing",
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  },
  "signing_address": {
    "id": "00000021A1442D8439B95C61432235892314",
    "company": "Starbucks",
    "address1": "56 W 9000 S",
    "address2": "",
    "city": "Sandy",
    "zone": {
      "id": "000CD01950BF7E14B11B7B50139023144915",
      "name": "Utah",
      "code": "UT",
      "timezone": "America/Denver"
    },
    "zipcode": "84070",
    "type": "signing",
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  },
  "escrow_number": "example",
  "closing_date": "example",
  "closing_time": null,
  "closing_timezone": null,
  "closing_time_mode": "time",
  "signing_type": 42,
  "signer_first_name": "example",
  "signer_last_name": "example",
  "signer_spouse_first_name": "example",
  "signer_spouse_last_name": "example",
  "cosigner_first_name": "example",
  "cosigner_last_name": "example",
  "cosigner_spouse_first_name": "example",
  "cosigner_spouse_last_name": "example",
  "contact_phone": "555-555-5555",
  "contact_phone_type": "example",
  "contact_altphone1": "555-555-5555",
  "contact_altphone1_type": "example",
  "contact_altphone2": "555-555-5555",
  "contact_altphone2_type": "example",
  "signer_email": "signers@email.com",
  "status": "ASSIGNED",
  "special_instructions": "Use Blue ink only",
  "contact_info": "Use Blue ink only",
  "return_instructions": "shipping label provided",
  "faxbacks": true,
  "document_url": "https://api.signingorder.com/orders/{(%2Fschemata%2Forder%23%2Fdefinitions%2Fidentity)}/documents",
  "created_at": "2015-01-01T12:00:00Z",
  "updated_at": "2015-01-01T12:00:00Z"
}
```

### <a name="link-GET-order-/orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}">Order Info</a>

Info for existing order.

```
GET /orders/{order_id}
```


#### Curl Example

```bash
$ curl -n https://api.signingorder.com/orders/$ORDER_ID
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
{
  "id": "01958EA1B308807443495AB1462573598756",
  "order_contact": {
    "id": "01958EA1B308807443495AB1462573598756",
    "name": "Joe Contact",
    "phone": "555-555-5555",
    "altphone": "444-444-4444",
    "address_book": true,
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  },
  "additional_contact": {
    "id": "01958EA1B308807443495AB1462573598756",
    "name": "Joe Contact",
    "phone": "555-555-5555",
    "address_book": true,
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  },
  "inhouse_contact": {
    "id": "01958EA1B308807443495AB1462573598756",
    "name": "Joe Contact",
    "phone": "555-555-5555",
    "address_book": true,
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  },
  "loan_officer": {
    "id": "01958EA1B308807443495AB1462573598756",
    "name": "Joe Contact",
    "phone": "555-555-5555",
    "address_book": true,
    "private": false,
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  },
  "property_address": {
    "id": "00000021A1442D8439B95C61432235892314",
    "company": "Starbucks",
    "address1": "56 W 9000 S",
    "address2": "",
    "city": "Sandy",
    "zone": {
      "id": "000CD01950BF7E14B11B7B50139023144915",
      "name": "Utah",
      "code": "UT",
      "timezone": "America/Denver"
    },
    "zipcode": "84070",
    "type": "signing",
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  },
  "signing_address": {
    "id": "00000021A1442D8439B95C61432235892314",
    "company": "Starbucks",
    "address1": "56 W 9000 S",
    "address2": "",
    "city": "Sandy",
    "zone": {
      "id": "000CD01950BF7E14B11B7B50139023144915",
      "name": "Utah",
      "code": "UT",
      "timezone": "America/Denver"
    },
    "zipcode": "84070",
    "type": "signing",
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  },
  "escrow_number": "example",
  "closing_date": "example",
  "closing_time": null,
  "closing_timezone": null,
  "closing_time_mode": "time",
  "signing_type": 42,
  "signer_first_name": "example",
  "signer_last_name": "example",
  "signer_spouse_first_name": "example",
  "signer_spouse_last_name": "example",
  "cosigner_first_name": "example",
  "cosigner_last_name": "example",
  "cosigner_spouse_first_name": "example",
  "cosigner_spouse_last_name": "example",
  "contact_phone": "555-555-5555",
  "contact_phone_type": "example",
  "contact_altphone1": "555-555-5555",
  "contact_altphone1_type": "example",
  "contact_altphone2": "555-555-5555",
  "contact_altphone2_type": "example",
  "signer_email": "signers@email.com",
  "status": "ASSIGNED",
  "special_instructions": "Use Blue ink only",
  "contact_info": "Use Blue ink only",
  "return_instructions": "shipping label provided",
  "faxbacks": true,
  "document_url": "https://api.signingorder.com/orders/{(%2Fschemata%2Forder%23%2Fdefinitions%2Fidentity)}/documents",
  "created_at": "2015-01-01T12:00:00Z",
  "updated_at": "2015-01-01T12:00:00Z"
}
```

### <a name="link-GET-order-/orders">Order List</a>

List existing orders.

```
GET /orders
```


#### Curl Example

```bash
$ curl -n https://api.signingorder.com/orders
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
[
  {
    "id": "01958EA1B308807443495AB1462573598756",
    "order_contact": {
      "id": "01958EA1B308807443495AB1462573598756",
      "name": "Joe Contact",
      "phone": "555-555-5555",
      "altphone": "444-444-4444",
      "address_book": true,
      "created_at": "2015-01-01T12:00:00Z",
      "updated_at": "2015-01-01T12:00:00Z"
    },
    "additional_contact": {
      "id": "01958EA1B308807443495AB1462573598756",
      "name": "Joe Contact",
      "phone": "555-555-5555",
      "address_book": true,
      "created_at": "2015-01-01T12:00:00Z",
      "updated_at": "2015-01-01T12:00:00Z"
    },
    "inhouse_contact": {
      "id": "01958EA1B308807443495AB1462573598756",
      "name": "Joe Contact",
      "phone": "555-555-5555",
      "address_book": true,
      "created_at": "2015-01-01T12:00:00Z",
      "updated_at": "2015-01-01T12:00:00Z"
    },
    "loan_officer": {
      "id": "01958EA1B308807443495AB1462573598756",
      "name": "Joe Contact",
      "phone": "555-555-5555",
      "address_book": true,
      "private": false,
      "created_at": "2015-01-01T12:00:00Z",
      "updated_at": "2015-01-01T12:00:00Z"
    },
    "property_address": {
      "id": "00000021A1442D8439B95C61432235892314",
      "company": "Starbucks",
      "address1": "56 W 9000 S",
      "address2": "",
      "city": "Sandy",
      "zone": {
        "id": "000CD01950BF7E14B11B7B50139023144915",
        "name": "Utah",
        "code": "UT",
        "timezone": "America/Denver"
      },
      "zipcode": "84070",
      "type": "signing",
      "created_at": "2015-01-01T12:00:00Z",
      "updated_at": "2015-01-01T12:00:00Z"
    },
    "signing_address": {
      "id": "00000021A1442D8439B95C61432235892314",
      "company": "Starbucks",
      "address1": "56 W 9000 S",
      "address2": "",
      "city": "Sandy",
      "zone": {
        "id": "000CD01950BF7E14B11B7B50139023144915",
        "name": "Utah",
        "code": "UT",
        "timezone": "America/Denver"
      },
      "zipcode": "84070",
      "type": "signing",
      "created_at": "2015-01-01T12:00:00Z",
      "updated_at": "2015-01-01T12:00:00Z"
    },
    "escrow_number": "example",
    "closing_date": "example",
    "closing_time": null,
    "closing_timezone": null,
    "closing_time_mode": "time",
    "signing_type": 42,
    "signer_first_name": "example",
    "signer_last_name": "example",
    "signer_spouse_first_name": "example",
    "signer_spouse_last_name": "example",
    "cosigner_first_name": "example",
    "cosigner_last_name": "example",
    "cosigner_spouse_first_name": "example",
    "cosigner_spouse_last_name": "example",
    "contact_phone": "555-555-5555",
    "contact_phone_type": "example",
    "contact_altphone1": "555-555-5555",
    "contact_altphone1_type": "example",
    "contact_altphone2": "555-555-5555",
    "contact_altphone2_type": "example",
    "signer_email": "signers@email.com",
    "status": "ASSIGNED",
    "special_instructions": "Use Blue ink only",
    "contact_info": "Use Blue ink only",
    "return_instructions": "shipping label provided",
    "faxbacks": true,
    "document_url": "https://api.signingorder.com/orders/{(%2Fschemata%2Forder%23%2Fdefinitions%2Fidentity)}/documents",
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  }
]
```

### <a name="link-PATCH-order-/orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}">Order Update</a>

Update an existing order.

```
PATCH /orders/{order_id}
```


#### Curl Example

```bash
$ curl -n -X PATCH https://api.signingorder.com/orders/$ORDER_ID \
  -d '{
}' \
  -H "Content-Type: application/json"
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
{
  "id": "01958EA1B308807443495AB1462573598756",
  "order_contact": {
    "id": "01958EA1B308807443495AB1462573598756",
    "name": "Joe Contact",
    "phone": "555-555-5555",
    "altphone": "444-444-4444",
    "address_book": true,
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  },
  "additional_contact": {
    "id": "01958EA1B308807443495AB1462573598756",
    "name": "Joe Contact",
    "phone": "555-555-5555",
    "address_book": true,
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  },
  "inhouse_contact": {
    "id": "01958EA1B308807443495AB1462573598756",
    "name": "Joe Contact",
    "phone": "555-555-5555",
    "address_book": true,
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  },
  "loan_officer": {
    "id": "01958EA1B308807443495AB1462573598756",
    "name": "Joe Contact",
    "phone": "555-555-5555",
    "address_book": true,
    "private": false,
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  },
  "property_address": {
    "id": "00000021A1442D8439B95C61432235892314",
    "company": "Starbucks",
    "address1": "56 W 9000 S",
    "address2": "",
    "city": "Sandy",
    "zone": {
      "id": "000CD01950BF7E14B11B7B50139023144915",
      "name": "Utah",
      "code": "UT",
      "timezone": "America/Denver"
    },
    "zipcode": "84070",
    "type": "signing",
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  },
  "signing_address": {
    "id": "00000021A1442D8439B95C61432235892314",
    "company": "Starbucks",
    "address1": "56 W 9000 S",
    "address2": "",
    "city": "Sandy",
    "zone": {
      "id": "000CD01950BF7E14B11B7B50139023144915",
      "name": "Utah",
      "code": "UT",
      "timezone": "America/Denver"
    },
    "zipcode": "84070",
    "type": "signing",
    "created_at": "2015-01-01T12:00:00Z",
    "updated_at": "2015-01-01T12:00:00Z"
  },
  "escrow_number": "example",
  "closing_date": "example",
  "closing_time": null,
  "closing_timezone": null,
  "closing_time_mode": "time",
  "signing_type": 42,
  "signer_first_name": "example",
  "signer_last_name": "example",
  "signer_spouse_first_name": "example",
  "signer_spouse_last_name": "example",
  "cosigner_first_name": "example",
  "cosigner_last_name": "example",
  "cosigner_spouse_first_name": "example",
  "cosigner_spouse_last_name": "example",
  "contact_phone": "555-555-5555",
  "contact_phone_type": "example",
  "contact_altphone1": "555-555-5555",
  "contact_altphone1_type": "example",
  "contact_altphone2": "555-555-5555",
  "contact_altphone2_type": "example",
  "signer_email": "signers@email.com",
  "status": "ASSIGNED",
  "special_instructions": "Use Blue ink only",
  "contact_info": "Use Blue ink only",
  "return_instructions": "shipping label provided",
  "faxbacks": true,
  "document_url": "https://api.signingorder.com/orders/{(%2Fschemata%2Forder%23%2Fdefinitions%2Fidentity)}/documents",
  "created_at": "2015-01-01T12:00:00Z",
  "updated_at": "2015-01-01T12:00:00Z"
}
```


## <a name="resource-order_contact">Order Contact</a>

Stability: `prototype`

An Order Contact

### Attributes

| Name | Type | Description | Example |
| ------- | ------- | ------- | ------- |
| **address_book** | *boolean* | in address book | `true` |
| **altphone** | *string* | alternate phone of order_contact<br/> **pattern:** `^\d{3}-\d{3}-\d{4}$` | `"444-444-4444"` |
| **created_at** | *date-time* | when order_contact was created | `"2015-01-01T12:00:00Z"` |
| **id** | *uuid* | unique identifier of order_contact | `"01958EA1B308807443495AB1462573598756"` |
| **name** | *string* | name of order_contact | `"Joe Contact"` |
| **phone** | *string* | phone of order_contact<br/> **pattern:** `^\d{3}-\d{3}-\d{4}$` | `"555-555-5555"` |
| **updated_at** | *date-time* | when order_contact was updated | `"2015-01-01T12:00:00Z"` |

### <a name="link-POST-order_contact-/order-contacts">Order Contact Create</a>

Create a new order contact in the address book.

```
POST /order-contacts
```

#### Optional Parameters

| Name | Type | Description | Example |
| ------- | ------- | ------- | ------- |
| **altphone** | *string* | alternate phone of order_contact<br/> **pattern:** `^\d{3}-\d{3}-\d{4}$` | `"444-444-4444"` |
| **email** | *email* | email of order_contact | `"order@contact.com"` |
| **name** | *string* | name of order_contact | `"Joe Contact"` |
| **phone** | *string* | phone of order_contact<br/> **pattern:** `^\d{3}-\d{3}-\d{4}$` | `"555-555-5555"` |


#### Curl Example

```bash
$ curl -n -X POST https://api.signingorder.com/order-contacts \
  -d '{
  "name": "Joe Contact",
  "email": "order@contact.com",
  "phone": "555-555-5555",
  "altphone": "444-444-4444"
}' \
  -H "Content-Type: application/json"
```


#### Response Example

```
HTTP/1.1 201 Created
```

```json
{
  "id": "01958EA1B308807443495AB1462573598756",
  "name": "Joe Contact",
  "phone": "555-555-5555",
  "altphone": "444-444-4444",
  "address_book": true,
  "created_at": "2015-01-01T12:00:00Z",
  "updated_at": "2015-01-01T12:00:00Z"
}
```

### <a name="link-DELETE-order_contact-/order-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}">Order Contact Delete</a>

Delete an existing order contact from the address book.

```
DELETE /order-contacts/{order_contact_id}
```


#### Curl Example

```bash
$ curl -n -X DELETE https://api.signingorder.com/order-contacts/$ORDER_CONTACT_ID \
  -H "Content-Type: application/json"
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
{
  "id": "01958EA1B308807443495AB1462573598756",
  "name": "Joe Contact",
  "phone": "555-555-5555",
  "altphone": "444-444-4444",
  "address_book": true,
  "created_at": "2015-01-01T12:00:00Z",
  "updated_at": "2015-01-01T12:00:00Z"
}
```

### <a name="link-GET-order_contact-/order-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}">Order Contact Info</a>

Info for existing order contact.

```
GET /order-contacts/{order_contact_id}
```


#### Curl Example

```bash
$ curl -n https://api.signingorder.com/order-contacts/$ORDER_CONTACT_ID
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
{
  "id": "01958EA1B308807443495AB1462573598756",
  "name": "Joe Contact",
  "phone": "555-555-5555",
  "altphone": "444-444-4444",
  "address_book": true,
  "created_at": "2015-01-01T12:00:00Z",
  "updated_at": "2015-01-01T12:00:00Z"
}
```

### <a name="link-PATCH-order_contact-/order-contacts/{(%23%2Fdefinitions%2Forder_contact%2Fdefinitions%2Fidentity)}">Order Contact Update</a>

Update an existing order contact.

```
PATCH /order-contacts/{order_contact_id}
```

#### Optional Parameters

| Name | Type | Description | Example |
| ------- | ------- | ------- | ------- |
| **altphone** | *string* | alternate phone of order_contact<br/> **pattern:** `^\d{3}-\d{3}-\d{4}$` | `"444-444-4444"` |
| **email** | *email* | email of order_contact | `"order@contact.com"` |
| **name** | *string* | name of order_contact | `"Joe Contact"` |
| **phone** | *string* | phone of order_contact<br/> **pattern:** `^\d{3}-\d{3}-\d{4}$` | `"555-555-5555"` |


#### Curl Example

```bash
$ curl -n -X PATCH https://api.signingorder.com/order-contacts/$ORDER_CONTACT_ID \
  -d '{
  "name": "Joe Contact",
  "email": "order@contact.com",
  "phone": "555-555-5555",
  "altphone": "444-444-4444"
}' \
  -H "Content-Type: application/json"
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
{
  "id": "01958EA1B308807443495AB1462573598756",
  "name": "Joe Contact",
  "phone": "555-555-5555",
  "altphone": "444-444-4444",
  "address_book": true,
  "created_at": "2015-01-01T12:00:00Z",
  "updated_at": "2015-01-01T12:00:00Z"
}
```


## <a name="resource-zone">Zones</a>

Stability: `prototype`

Zones represent a State

### Attributes

| Name | Type | Description | Example |
| ------- | ------- | ------- | ------- |
| **code** | *string* | 2 character code of zone | `"UT"` |
| **id** | *uuid* | unique identifier | `"000CD01950BF7E14B11B7B50139023144915"` |
| **name** | *string* | name of zone | `"Utah"` |
| **timezone** | *string* | IANA format timezone identifier | `"America/Denver"` |

### <a name="link-GET-zone-/zones/{(%23%2Fdefinitions%2Fzone%2Fdefinitions%2Fidentity)}">Zones Info</a>

Info for existing zone.

```
GET /zones/{zone_id_or_name_or_code}
```


#### Curl Example

```bash
$ curl -n https://api.signingorder.com/zones/$ZONE_ID_OR_NAME_OR_CODE
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
{
  "id": "000CD01950BF7E14B11B7B50139023144915",
  "name": "Utah",
  "code": "UT",
  "timezone": "America/Denver"
}
```

### <a name="link-GET-zone-/zones">Zones List</a>

List existing zones.

```
GET /zones
```


#### Curl Example

```bash
$ curl -n https://api.signingorder.com/zones
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
[
  {
    "id": "000CD01950BF7E14B11B7B50139023144915",
    "name": "Utah",
    "code": "UT",
    "timezone": "America/Denver"
  }
]
```


