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
| **created_at** | *date-time* | when order was created | `"2015-01-01T12:00:00Z"` |
| **id** | *uuid* | unique identifier of order | `"01234567-89ab-cdef-0123-456789abcdef"` |
| **name** | *string* | unique name of order | `"example"` |
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
  "created_at": "2015-01-01T12:00:00Z",
  "id": "01234567-89ab-cdef-0123-456789abcdef",
  "name": "example",
  "updated_at": "2015-01-01T12:00:00Z"
}
```

### <a name="link-DELETE-order-/orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}">Order Delete</a>

Delete an existing order.

```
DELETE /orders/{order_id_or_name}
```


#### Curl Example

```bash
$ curl -n -X DELETE https://api.signingorder.com/orders/$ORDER_ID_OR_NAME \
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
  "name": "example",
  "updated_at": "2015-01-01T12:00:00Z"
}
```

### <a name="link-GET-order-/orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}">Order Info</a>

Info for existing order.

```
GET /orders/{order_id_or_name}
```


#### Curl Example

```bash
$ curl -n https://api.signingorder.com/orders/$ORDER_ID_OR_NAME
```


#### Response Example

```
HTTP/1.1 200 OK
```

```json
{
  "created_at": "2015-01-01T12:00:00Z",
  "id": "01234567-89ab-cdef-0123-456789abcdef",
  "name": "example",
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
    "created_at": "2015-01-01T12:00:00Z",
    "id": "01234567-89ab-cdef-0123-456789abcdef",
    "name": "example",
    "updated_at": "2015-01-01T12:00:00Z"
  }
]
```

### <a name="link-PATCH-order-/orders/{(%23%2Fdefinitions%2Forder%2Fdefinitions%2Fidentity)}">Order Update</a>

Update an existing order.

```
PATCH /orders/{order_id_or_name}
```


#### Curl Example

```bash
$ curl -n -X PATCH https://api.signingorder.com/orders/$ORDER_ID_OR_NAME \
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
  "created_at": "2015-01-01T12:00:00Z",
  "id": "01234567-89ab-cdef-0123-456789abcdef",
  "name": "example",
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


