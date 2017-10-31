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
