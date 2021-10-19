# Open API - Programmable Cards API

The Programmable card API API allows Investec SA Private Banking clients to access their programmable card functionality via an API.

It can be used to retrieve account information such as:

* Obtain a list of cards
* Get code saved to the card
* Get code published to the card
* Save code to the card
* Publish code to the card
* Execute (Simulation)
* Get all executions (Simulations and Actual)
* Get environment variables
* Overwrite environment variables
* Get countries
* Get currencies
* Get merchant categories

## Release Notes (Cards)
<hr>

## Get Cards
`GET /za/pb/v1/cards`

Obtain cards associated with the account.

> Example Request

```http
GET https://openapi.investec.com/za/pb/v1/cards HTTP/1.1
Authorization: Bearer Ms9OsZkyrhBZd5yQJgfEtiDy4t2c
Host: openapi.investec.com
Accept: application/json
```

```javascript
$.ajax({
   url: 'https://openapi.investec.com/za/pb/v1/cards',
   type: 'GET',
   headers: {
      'Authorization': 'Bearer Ms9OsZkyrhBZd5yQJgfEtiDy4t2c'
   },
   success: function (result) {
       console.log(JSON.stringify(result));
   }
});
```
> Example Response

```json
{
    "data": {
        "cards": [
            {
                "CardKey": "1010101",
                "CardNumber": "402167XXXXXX1111",
                "IsProgrammable": true,
                "Status": "Active",
                "CardTypeCode": "VGC",
                "AccountNumber": "10011001100",
                "AccountId": "177778438355553632224",
            },
            {
                "CardKey": "1020202",
                "CardNumber": "402167XXXXXX2222",
                "IsProgrammable": true,
                "Status": "Active",
                "CardTypeCode": "VGC",
                "AccountNumber": "10011001100",
                "AccountId": "177778438355553632224",
            },
            {
                "CardKey": "1030303",
                "CardNumber": "402167XXXXXX3333",
                "IsProgrammable": true,
                "Status": "Active",
                "CardTypeCode": "VGC",
                "AccountNumber": "10011001100",
                "AccountId": "177778438355553632224",
            }
        ]
    },
    "links": {
        "self": null
    },
    "meta": {
        "totalPages": 1
    }
}

```
### HTTP Status Codes

HTTP Status | Description
-|-
200 - OK | The requested operation was successfully completed
400 - Bad Request | The requested operation will not be carried out
401 - Unauthorized | The requested operation was refused access
429 - Too Many Requests | To many requests in quick succession, Retry the request.
500 - Internal Server Error | The requested operation failed to execute

## Get Function (saved) code

`GET /za/pb/v1/cards/{cardkey}/code`

Obtain code currently saved to the specific card.

>Example Request:

```http
GET https://openapi.investec.com/za/pb/v1/cards/{cardkey}/code HTTP/1.1
Authorization: Bearer Ms9OsZkyrhBZd5yQJgfEtiDy4t2c
Host: openapi.investec.com
Accept: application/json
```

```javascript
$.ajax({
   url: 'https://openapi.investec.com/za/pb/v1/cards/{cardkey}/code',
   type: 'GET',
   headers: {
      'Authorization': 'Bearer Ms9OsZkyrhBZd5yQJgfEtiDy4t2c'
   },
   success: function (result) {
       console.log(JSON.stringify(result));
   }
});
```

> Example Response

```json
{
    "data": {
        "result": {
            "codeId": "3BB77753-R2D2-4U2B-1A2B-4C213E7D0AC3",
            "code": "// This function runs during the card transaction authorization flow.\n// It has a limited execution time, so keep any code short-running.\nconst beforeTransaction = async (authorization) => {\n    console.log(authorization);\n    return true;\n};\n\n// This function runs after a transaction.\nconst afterTransaction = async (transaction) => {\n    console.log(transaction)\n};\n\n// This function runs after a transaction.\nconst afterDecline = async (transaction) => {\n    console.log(transaction)\n};",
            "createdAt": "2021-10-12T09:12:06.695Z",
            "updatedAt": "2021-10-12T09:12:06.695Z",
            "publishedAt": "2021-10-12T09:12:16.557Z",
            "error": null
        }
    },
    "links": {
        "self": null
    },
    "meta": {
        "totalPages": 1
    }
}
```
### Parameters

Name | In | Type | Required | Description
-|-|-|-|-
cardkey | query | string | required | The CardKey obtained from the get cards call.

### HTTP Status Codes

HTTP Status | Description
-|-
200 - OK | The requested operation was successfully completed
400 - Bad Request | The requested operation will not be carried out
401 - Unauthorized | The requested operation was refused access
429 - Too Many Requests | To many requests in quick succession, Retry the request.
500 - Internal Server Error | The requested operation failed to execute

## Get Function (published) code

<a name="get-function-code">
`GET /za/pb/v1/cards/{cardkey}/publishedcode`</a>

Obtain code currently published to the specific card.

>Example Request:

```http
GET https://openapi.investec.com/za/pb/v1/cards/{cardkey}/publishedcode HTTP/1.1
Authorization: Bearer Ms9OsZkyrhBZd5yQJgfEtiDy4t2c
Host: openapi.investec.com
Accept: application/json
```

```javascript
$.ajax({
   url: 'https://openapi.investec.com/za/pb/v1/cards/{card_id}/publishedcode',
   type: 'GET',
   headers: {
      'Authorization': 'Bearer Ms9OsZkyrhBZd5yQJgfEtiDy4t2c'
   },
   success: function (result) {
       console.log(JSON.stringify(result));
   }
});
```

> Example Response

``` json
{
    "data": {
        "result": {
            "codeId": "3BB77753-R2D2-4U2B-1A2B-4C213E7D0AC3",
            "code": "// This function runs during the card transaction authorization flow.\n// It has a limited execution time, so keep any code short-running.\nconst beforeTransaction = async (authorization) => {\n    console.log(authorization);\n    return true;\n};\n\n// This function runs after a transaction.\nconst afterTransaction = async (transaction) => {\n    console.log(transaction)\n};\n\n// This function runs after a transaction.\nconst afterDecline = async (transaction) => {\n    console.log(transaction)\n};",
            "createdAt": "2021-10-12T09:12:06.695Z",
            "updatedAt": "2021-10-12T09:12:06.695Z",
            "publishedAt": "2021-10-12T09:12:16.557Z",
            "error": null
        }
    },
    "links": {
        "self": null
    },
    "meta": {
        "totalPages": 1
    }
}
```
### Parameters

Name | In | Type | Required | Description
-|-|-|-|-
cardkey | query | string | required | The CardKey obtained from the get cards call.

### HTTP Status Codes

HTTP Status | Description
-|-
200 - OK | The requested operation was successfully completed
400 - Bad Request | The requested operation will not be carried out
401 - Unauthorized | The requested operation was refused access
429 - Too Many Requests | To many requests in quick succession, Retry the request.
500 - Internal Server Error | The requested operation failed to execute

## Update Function Code (Without publishing to card)

`POST /za/pb/v1/cards/{cardkey}/code`

Save specified code to the specific card.  
*Note: This allows you to save/stage the code to the card without publishing it. This implies that the code will not execute when a card transaction occurs.*
>Example Request:

```http
POST https://openapi.investec.com/za/pb/v1/cards/{cardkey}/code HTTP/1.1
Authorization: Bearer Ms9OsZkyrhBZd5yQJgfEtiDy4t2c
Host: openapi.investec.com
Accept: application/json

{
    "code": "// This function runs during the card transaction authorization flow.\n// It has a limited execution time, so keep any code short-running.\nconst beforeTransaction = async (authorization) => {\n    console.log(authorization);\n    return true;\n};\n\n// This function runs after a transaction.\nconst afterTransaction = async (transaction) => {\n    console.log(transaction)\n};\n\n// This function runs after a transaction.\nconst afterDecline = async (transaction) => {\n    console.log(transaction)\n};"
}
```

```javascript
$.ajax({
   url: 'https://openapi.investec.com/za/pb/v1/cards/{cardkey}/code',
   type: 'POST',
   headers: {
      'Authorization': 'Bearer Ms9OsZkyrhBZd5yQJgfEtiDy4t2c'
   },
   data: {
    "code": "// This function runs during the card transaction authorization flow.\n// It has a limited execution time, so keep any code short-running.\nconst beforeTransaction = async (authorization) => {\n    console.log(authorization);\n    return true;\n};\n\n// This function runs after a transaction.\nconst afterTransaction = async (transaction) => {\n    console.log(transaction)\n};\n\n// This function runs after a transaction.\nconst afterDecline = async (transaction) => {\n    console.log(transaction)\n};"
  }
   success: function (result) {
       console.log(JSON.stringify(result));
   }
});
```

> Example Response

```json
{
    "data": {
        "result": {
            "codeId": "3BB77753-R2D2-4U2B-1A2B-4C213E7D0AC3",
            "code": "// This function runs during the card transaction authorization flow.\n// It has a limited execution time, so keep any code short-running.\nconst beforeTransaction = async (authorization) => {\n    console.log(authorization);\n    return true;\n};\n\n// This function runs after a transaction.\nconst afterTransaction = async (transaction) => {\n    console.log(transaction)\n};\n\n// This function runs after a transaction.\nconst afterDecline = async (transaction) => {\n    console.log(transaction)\n};",
            "createdAt": "2021-10-12T09:12:06.695Z",
            "updatedAt": "2021-10-12T09:12:06.695Z",
            "publishedAt": "2021-10-12T09:12:16.557Z",
            "error": null
        }
    },
    "links": {
        "self": null
    },
    "meta": {
        "totalPages": 1
    }
}
```
### Parameters

Name | In | Type | Required | Description
-|-|-|-|-
cardkey | query | string | required | The CardKey obtained from the get cards call.
code | body | string | required | The javascript code that will be published to the card.

### HTTP Status Codes

HTTP Status | Description
-|-
200 - OK | The requested operation was successfully completed
400 - Bad Request | The requested operation will not be carried out
401 - Unauthorized | The requested operation was refused access
429 - Too Many Requests | To many requests in quick succession, Retry the request.
500 - Internal Server Error | The requested operation failed to execute

## Publish Function Code

`POST /za/pb/v1/cards/{cardkey}/publish`

Publish specified code to the specific card.  
*Note: This allows you to push code to the specified card. After successfully publishing the code it will execute the next time a card transaction occurs.*
>Example Request:

```http
POST https://openapi.investec.com/za/pb/v1/cards/{cardkey}/publish HTTP/1.1
Authorization: Bearer Ms9OsZkyrhBZd5yQJgfEtiDy4t2c
Host: openapi.investec.com
Accept: application/json

{
    "codeid": "{codeid}",
    "code": "// This function runs during the card transaction authorization flow.\n// It has a limited execution time, so keep any code short-running.\nconst beforeTransaction = async (authorization) => {\n    console.log(authorization);\n    return true;\n};\n\n// This function runs after a transaction.\nconst afterTransaction = async (transaction) => {\n    console.log(transaction)\n};\n\n// This function runs after a transaction.\nconst afterDecline = async (transaction) => {\n    console.log(transaction)\n};"
}
```

```javascript
$.ajax({
   url: 'https://openapi.investec.com/za/pb/v1/cards/{cardkey}/publish',
   type: 'POST',
   headers: {
      'Authorization': 'Bearer Ms9OsZkyrhBZd5yQJgfEtiDy4t2c'
   },
   data: {
    "codeid": "{codeid}",
    "code": "// This function runs during the card transaction authorization flow.\n// It has a limited execution time, so keep any code short-running.\nconst beforeTransaction = async (authorization) => {\n    console.log(authorization);\n    return true;\n};\n\n// This function runs after a transaction.\nconst afterTransaction = async (transaction) => {\n    console.log(transaction)\n};\n\n// This function runs after a transaction.\nconst afterDecline = async (transaction) => {\n    console.log(transaction)\n};"
  }
   success: function (result) {
       console.log(JSON.stringify(result));
   }
});
```

> Example Response
``` json
{
    "data": {
        "result": {
            "codeId": "3BB77753-R2D2-4U2B-1A2B-4C213E7D0AC3",
            "code": "// This function runs during the card transaction authorization flow.\n// It has a limited execution time, so keep any code short-running.\nconst beforeTransaction = async (authorization) => {\n    console.log(authorization);\n    return true;\n};\n\n// This function runs after a transaction.\nconst afterTransaction = async (transaction) => {\n    console.log(transaction)\n};\n\n// This function runs after a transaction.\nconst afterDecline = async (transaction) => {\n    console.log(transaction)\n};",
            "createdAt": "2021-10-12T09:12:06.695Z",
            "updatedAt": "2021-10-12T09:12:06.695Z",
            "publishedAt": "2021-10-12T09:12:16.557Z",
            "error": null
        }
    },
    "links": {
        "self": null
    },
    "meta": {
        "totalPages": 1
    }
}
```
### Parameters

Name | In | Type | Required | Description
-|-|-|-|-
cardkey | query | string | required | The CardKey obtained from the get cards call.
codeid | body | string | required | Refers to the codeid of the published code which can be obtained from the [Get Published Code](#get-function-code) endpoint.
code | body | string | required | The javascript code that will be published to the card.

### HTTP Status Codes

HTTP Status | Description
-|-
200 - OK | The requested operation was successfully completed
400 - Bad Request | The requested operation will not be carried out
401 - Unauthorized | The requested operation was refused access
429 - Too Many Requests | To many requests in quick succession, Retry the request.
500 - Internal Server Error | The requested operation failed to execute

## Execute Function Code (Simulation)

`POST /za/pb/v1/cards/{cardkey}/code/execute`

Publish specified code to the specific card.  
*Note: This allows you to push code to the specified card. After successfully publishing the code it will execute the next time a card transaction occurs.*
>Example Request:

```http
POST https://openapi.investec.com/za/pb/v1/cards/{cardkey}/execute HTTP/1.1
Authorization: Bearer Ms9OsZkyrhBZd5yQJgfEtiDy4t2c
Host: openapi.investec.com
Accept: application/json

{
    "simulationcode": "// This function runs during the card transaction authorization flow.\n// It has a limited execution time, so keep any code short-running.\nconst beforeTransaction = async (authorization) => {\n    console.log(authorization);\n    return true;\n};\n\n// This function runs after a transaction.\nconst afterTransaction = async (transaction) => {\n    console.log(transaction)\n};\n\n// This function runs after a transaction.\nconst afterDecline = async (transaction) => {\n    console.log(transaction)\n};",
    "centsAmount": "10100",
    "currencyCode": "zar",
    "merchantCode": 7996,
    "merchantCity": "Durban",
    "countryCode": "ZA"
}
```

```javascript
$.ajax({
   url: 'https://openapi.investec.com/za/pb/v1/cards/{cardkey}/execute',
   type: 'POST',
   headers: {
      'Authorization': 'Bearer Ms9OsZkyrhBZd5yQJgfEtiDy4t2c'
   },
   data: {
    "code": "// This function runs during the card transaction authorization flow.\n// It has a limited execution time, so keep any code short-running.\nconst beforeTransaction = async (authorization) => {\n    console.log(authorization);\n    return true;\n};\n\n// This function runs after a transaction.\nconst afterTransaction = async (transaction) => {\n    console.log(transaction)\n};\n\n// This function runs after a transaction.\nconst afterDecline = async (transaction) => {\n    console.log(transaction)\n};",
    "centsAmount": "10100",
    "currencyCode": "zar",
    "merchantCode": 7996,
    "merchantCity": "Durban",
    "countryCode": "ZA"
  }
   success: function (result) {
       console.log(JSON.stringify(result));
   }
});
```

> Example Response

``` json
{
    "data": {
        "result": [
            {
                "executionId": "ABB8854D-B3A4-4450-BB9D-915206E8D156",
                "rootCodeFunctionId": "96A88E6A-E343-4051-9674-7C58B4CC1A58",
                "sandbox": true,
                "type": "before_transaction",
                "authorizationApproved": true,
                "logs": [
                    {
                        "createdAt": "2021-10-15T03:48:14.771Z",
                        "level": "info",
                        "content": "{\"accountNumber\":\"10011001100\",\"dateTime\":\"2021-10-15T03:48:13.147Z\",\"centsAmount\":10100,\"currencyCode\":\"zar\",\"type\":\"card\",\"reference\":\"simulation\",\"card\":{\"id\":\"1010101\"},\"merchant\":{\"category\":{\"code\":\"7996\",\"key\":\"amusement_parks_carnivals\",\"name\":\"Amusement Parks/Carnivals\"},\"name\":\"uShaka Marine World\",\"city\":\"Durban\",\"country\":{\"code\":\"ZA\",\"alpha3\":\"ZAF\",\"name\":\"South Africa\"}}}"
                    }
                ],
                "smsCount": 0,
                "emailCount": 0,
                "pushNotificationCount": 0,
                "createdAt": "2021-10-15T03:48:14.184Z",
                "startedAt": "2021-10-15T03:48:14.183Z",
                "completedAt": "2021-10-15T03:48:14.899Z",
                "updatedAt": "2021-10-15T03:48:14.184Z",
                "Error": null
            },
            {
                "executionId": "688357B2-1F2B-49F7-A739-3D9DAF87CD84",
                "rootCodeFunctionId": "96A88E6A-E343-4051-9674-7C58B4CC1A58",
                "sandbox": true,
                "type": "after_transaction",
                "authorizationApproved": null,
                "logs": [
                    {
                        "createdAt": "2021-10-15T03:48:15.712Z",
                        "level": "info",
                        "content": "{\"accountNumber\":\"10011001100\",\"dateTime\":\"2021-10-15T03:48:14.920Z\",\"centsAmount\":10100,\"currencyCode\":\"zar\",\"type\":\"card\",\"reference\":\"simulation\",\"card\":{\"id\":\"1010101\"},\"merchant\":{\"category\":{\"code\":\"7996\",\"key\":\"amusement_parks_carnivals\",\"name\":\"Amusement Parks/Carnivals\"},\"name\":\"uShaka Marine World\",\"city\":\"Durban\",\"country\":{\"code\":\"ZA\",\"alpha3\":\"ZAF\",\"name\":\"South Africa\"}}}"
                    }
                ],
                "smsCount": 0,
                "emailCount": 0,
                "pushNotificationCount": 0,
                "createdAt": "2021-10-15T03:48:15.082Z",
                "startedAt": "2021-10-15T03:48:15.082Z",
                "completedAt": "2021-10-15T03:48:15.826Z",
                "updatedAt": "2021-10-15T03:48:15.082Z",
                "Error": null
            }
        ]
    },
    "links": {
        "self": null
    },
    "meta": {
        "totalPages": 1
    }
}
```
### Parameters

Name | In | Type | Required | Description
-|-|-|-|-
simulationcode | body | string | required | The javascript code that will be published to the card.

### HTTP Status Codes

HTTP Status | Description
-|-
200 - OK | The requested operation was successfully completed
400 - Bad Request | The requested operation will not be carried out
401 - Unauthorized | The requested operation was refused access
429 - Too Many Requests | To many requests in quick succession, Retry the request.
500 - Internal Server Error | The requested operation failed to execute

## Get Executions
`GET za/v1/cards/{cardkey}/code/executions`
Fetches the logs of the simulated as well as the actual transactions for the spesific card

>Example Request:

```http
GET /za/v1/cards/{cardkey}/code/executions HTTP/1.1
Host: openapi.investec.com
Content-Type: application/json
Authorization: Bearer V61dgYGhIFfr8NAkMwzJcZbeeiVZ
```

```javascript
$.ajax({
   url: 'https://openapi.investec.com/za/v1/cards/{cardkey}/code/executions',
   type: 'GET',
   headers: {
      'Authorization': 'Bearer Ms9OsZkyrhBZd5yQJgfEtiDy4t2c'
   }
   success: function (result) {
       console.log(JSON.stringify(result));
   }
});
```
> Example Response

``` json
{
    "data": {
        "result": {
            "executionItems": [
                {
                    "executionId": "5514C8A1-63BB-48FF-979A-F56660510FEC",
                    "rootCodeFunctionId": "96A88E6A-E343-4051-9674-7C58B4CC1A58",
                    "sandbox": true,
                    "type": "after_transaction",
                    "authorizationApproved": null,
                    "logs": [
                        {
                            "createdAt": "2021-10-14T12:55:29.814Z",
                            "level": "info",
                            "content": "{\"accountNumber\":\"10011001100\",\"dateTime\":\"2021-10-14T12:55:30.000Z\",\"centsAmount\":63943,\"currencyCode\":\"zar\",\"type\":\"card\",\"reference\":\"51387120\",\"card\":{\"id\":\"1010101\",\"display\":\"402167XXXXXX1111\"},\"merchant\":{\"category\":{\"code\":\"5912\",\"key\":\"drug_stores_and_pharmacies\",\"name\":\"Drug Stores and Pharmacies\"},\"name\":\"CORNERSTONE PHARMACY H\",\"city\":\"ROODEPOORT\",\"country\":{\"code\":\"ZA\",\"alpha3\":\"ZAF\",\"name\":\"South Africa\"}}}"
                        }
                    ],
                    "smsCount": 0,
                    "emailCount": 0,
                    "pushNotificationCount": 0,
                    "createdAt": "2021-10-14T12:55:29.118Z",
                    "startedAt": "2021-10-14T12:55:29.118Z",
                    "completedAt": "2021-10-14T12:55:29.858Z",
                    "updatedAt": "2021-10-14T12:55:29.118Z"
                },
                {
                    "executionId": "FECF32DE-77E6-4EEF-918E-E536B093336A",
                    "rootCodeFunctionId": "96A88E6A-E343-4051-9674-7C58B4CC1A58",
                    "sandbox": true,
                    "type": "before_transaction",
                    "authorizationApproved": null,
                    "logs": [
                        {
                            "createdAt": "2021-10-14T12:55:27.354Z",
                            "level": "info",
                            "content": "{\"accountNumber\":\"10011001100\",\"dateTime\":\"2021-10-14T12:55:28.000Z\",\"centsAmount\":63943,\"currencyCode\":\"zar\",\"type\":\"card\",\"reference\":\"51387120\",\"card\":{\"id\":\"1010101\",\"display\":\"402167XXXXXX1111\"},\"merchant\":{\"category\":{\"code\":\"5912\",\"key\":\"drug_stores_and_pharmacies\",\"name\":\"Drug Stores and Pharmacies\"},\"name\":\"CORNERSTONE PHARMACY H\",\"city\":\"ROODEPOORT\",\"country\":{\"code\":\"ZA\",\"alpha3\":\"ZAF\",\"name\":\"South Africa\"}}}"
                        }
                    ],
                    "smsCount": 0,
                    "emailCount": 0,
                    "pushNotificationCount": 0,
                    "createdAt": "2021-10-14T12:55:26.679Z",
                    "startedAt": "2021-10-14T12:55:26.679Z",
                    "completedAt": "2021-10-14T12:55:27.394Z",
                    "updatedAt": "2021-10-14T12:55:26.679Z"
                }
            ],
            "error": null
        }
    },
    "links": {
        "self": null
    },
    "meta": {
        "totalPages": 1
    }
}
```

### Parameters
Name | In | Type | Required | Description
-|-|-|-|-
cardkey | query | string | required | The CardKey obtained from the get cards call.

### HTTP Status Codes
HTTP Status | Description
-|-
200 - OK | The requested operation was successfully completed
400 - Bad Request | The requested operation will not be carried out
401 - Unauthorized | The requested operation was refused access
429 - Too Many Requests | To many requests in quick succession, Retry the request.
500 - Internal Server Error | The requested operation failed to execute

## Get Environment Variables
`GET /za/v1/cards/{cardkey}/environmentvariables`
Gets the key value pairs of user defined variables securely stored against a spesific card
>Example Request:

```http
GET /za/v1/cards/{cardkey}/environmentvariables HTTP/1.1
Host: openapi.investec.com
Content-Type: application/json
Authorization: Bearer V61dgYGhIFfr8NAkMwzJcZbeeiVZ
```

```javascript
$.ajax({
   url: 'https://openapi.investec.com/za/v1/cards/{cardkey}/environmentvariables',
   type: 'GET',
   headers: {
      'Authorization': 'Bearer Ms9OsZkyrhBZd5yQJgfEtiDy4t2c'
   }
   success: function (result) {
       console.log(JSON.stringify(result));
   }
});
```
> Example Response

``` json
{
    "data": {
        "result": {
            "variables": {},
            "createdAt": "2021-10-12T09:12:06.742Z",
            "updatedAt": "2021-10-12T09:12:06.742Z",
            "error": null
        }
    },
    "links": {
        "self": null
    },
    "meta": {
        "totalPages": 1
    }
}
```
### Parameters

Name | In | Type | Required | Description
-|-|-|-|-
cardkey | query | string | required | The CardKey obtained from the get cards call.

### HTTP Status Codes
HTTP Status | Description
-|-
200 - OK | The requested operation was successfully completed
400 - Bad Request | The requested operation will not be carried out
401 - Unauthorized | The requested operation was refused access
429 - Too Many Requests | To many requests in quick succession, Retry the request.
500 - Internal Server Error | The requested operation failed to execute

## Replace Environment Variables
`POST /za/v1/cards/{cardkey}/environmentvariables`
Sets the environment variables stored agains a spesific card.  
*Note: This replaces all variables and does not allow for patching*

>Example Request:

```http
POST /za/v1/cards/{cardkey}/environmentvariables HTTP/1.1
Host: openapi.investec.com
Content-Type: application/json
Authorization: Bearer 8WqIBAVADiQxwtl8GvE1cAZAQhNK
Content-Length: 92

{
    "variables": {
        "test1": "value11",
        "test2": "value22"
        }
}
```

```javascript
$.ajax({
   url: 'https://openapi.investec.com/za/v1/cards/{cardkey}/environmentvariables',
   type: 'POST',
   headers: {
      'Authorization': 'Bearer Ms9OsZkyrhBZd5yQJgfEtiDy4t2c'
   },
   data: {
    "variables": {
        "test1": "value11",
        "test2": "value22"
        }
    }   
   success: function (result) {
       console.log(JSON.stringify(result));
   }
});
```
> Example Response

``` json
{
    "data": {
        "result": {
            "variables": {
                "test1": "value11",
                "test2": "value22"
            },
            "createdAt": "2021-10-15T05:18:09.891Z",
            "updatedAt": "2021-10-15T05:18:09.891Z",
            "error": null
        }
    },
    "links": {
        "self": null
    },
    "meta": {
        "totalPages": 1
    }
}
```
### Parameters

Name | In | Type | Required | Description
-|-|-|-|-
cardkey | query | string | required | The CardKey obtained from the get cards call.
variables | body | string | optional | A json object representing the key/value pairs

### HTTP Status Codes
HTTP Status | Description
-|-
200 - OK | The requested operation was successfully completed
400 - Bad Request | The requested operation will not be carried out
401 - Unauthorized | The requested operation was refused access
429 - Too Many Requests | To many requests in quick succession, Retry the request.
500 - Internal Server Error | The requested operation failed to execute

  
## Get Countries
`GET /za/v1/cards/countries`
Gets a reference set of countries 
>Example Request:

```http
GET /za/v1/cards/countries HTTP/1.1
Host: openapi.investec.com
Authorization: Bearer S4gK4r6Llqvt0AwEOMron3AHkl6u
```

```javascript
var settings = {
  "url": "https://openapi.investec.com/za/v1/cards/countries",
  "method": "GET",
  "timeout": 0,
  "headers": {
    "Authorization": "Bearer S4gK4r6Llqvt0AwEOMron3AHkl6u"
  },
};

$.ajax(settings).done(function (response) {
  console.log(response);
});
```
> Example Response

``` json
{
    "data": {
        "result": [
            {
                "Code": "ZA",
                "Name": "South Africa"
            },
            {
                "Code": "GB",
                "Name": "United Kingdom of Great Britain and Northern Ireland (the)"
            },
            {
                "Code": "undefined",
                "Name": "undefined"
            }
        ]
    },
    "links": {
        "self": null
    },
    "meta": {
        "totalPages": 1
    }
}
```

### HTTP Status Codes
HTTP Status | Description
-|-
200 - OK | The requested operation was successfully completed
400 - Bad Request | The requested operation will not be carried out
401 - Unauthorized | The requested operation was refused access
429 - Too Many Requests | To many requests in quick succession, Retry the request.
500 - Internal Server Error | The requested operation failed to execute

## Get Currencies
`GET /za/v1/cards/currencies`
Gets a reference set of currencies
>Example Request:

```http
GET /za/v1/cards/currencies HTTP/1.1
Host: openapi.investec.com
Authorization: Bearer S4gK4r6Llqvt0AwEOMron3AHkl6u
```

```javascript
var settings = {
  "url": "https://openapi.investec.com/za/v1/cards/currencies",
  "method": "GET",
  "timeout": 0,
  "headers": {
    "Authorization": "Bearer S4gK4r6Llqvt0AwEOMron3AHkl6u"
  },
};

$.ajax(settings).done(function (response) {
  console.log(response);
});
```
> Example Response

``` json
{
    "data": {
        "result": [
            {
                "Code": "ZAR",
                "Name": "South African Rand"
            },
            {
                "Code": "GBP",
                "Name": "British Pound"
            },
            {
                "Code": "USD",
                "Name": "United States Dollar"
            },
            {
                "Code": "EUR",
                "Name": "Euro"
            }
        ]
    },
    "links": {
        "self": null
    },
    "meta": {
        "totalPages": 1
    }
}
```

### HTTP Status Codes
HTTP Status | Description
-|-
200 - OK | The requested operation was successfully completed
400 - Bad Request | The requested operation will not be carried out
401 - Unauthorized | The requested operation was refused access
429 - Too Many Requests | To many requests in quick succession, Retry the request.
500 - Internal Server Error | The requested operation failed to execute

## Get Merchants
`GET /za/v1/cards/merchants`
Get a reference set of merchant category codes and descriptions
>Example Request:

```http
GET /za/v1/cards/merchants HTTP/1.1
Host: openapi.investec.com
Authorization: Bearer S4gK4r6Llqvt0AwEOMron3AHkl6u
```

```javascript
var settings = {
  "url": "https://openapi.investec.com/za/v1/cards/merchants",
  "method": "GET",
  "timeout": 0,
  "headers": {
    "Authorization": "Bearer S4gK4r6Llqvt0AwEOMron3AHkl6u"
  },
};

$.ajax(settings).done(function (response) {
  console.log(response);
});
```
> Example Response

``` json
{
    "data": {
        "result": [
            {
                "Code": "7623",
                "Name": "A/C, Refrigeration Repair"
            },
            {
                "Code": "8931",
                "Name": "Accounting/Bookkeeping Services"
            },
            {
                "Code": "7311",
                "Name": "Advertising Services"
            }
        ]
    },
    "links": {
        "self": null
    },
    "meta": {
        "totalPages": 1
    }
}
```

### HTTP Status Codes
HTTP Status | Description
-|-
200 - OK | The requested operation was successfully completed
400 - Bad Request | The requested operation will not be carried out
401 - Unauthorized | The requested operation was refused access
429 - Too Many Requests | To many requests in quick succession, Retry the request.
500 - Internal Server Error | The requested operation failed to execute