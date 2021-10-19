# Open API

## Introduction

The Account Information API allows Investec SA Private Banking clients to access their account and transactional information (read-only) via an API.

It can be used to retrieve account information such as:

* Account details
* Balances
* Account transactions

There are many possible use cases for the Account Information API: from extracting the data to aggregate it with financial data from other banking institutions to personal money management tools.

## Release Notes

### 2020-11-13

* **Enhancement**: Included transaction type filter to <a href="#get-account-transactions">Get Account Transactions</a>

### 2020-11-10

* **Enhancement**: Included postedOrder to <a href="#get-account-transactions">Get Account Transactions</a>
* **Enhancement**: Included transactionType to <a href="#get-account-transactions">Get Account Transactions</a>

### 2020-09-08

* **Fix**: Implemented CORS support
* **Fix**: Implemented multi User-Agent support

### 2020-07-21

* **Enhancement**: Included transactionDate to <a href="#get-account-transactions">Get Account Transactions</a>
* **Enhancement**: Included date range filter to <a href="#get-account-transactions">Get Account Transactions</a>

### 2021-10-01

<hr>

## Authorization

The Account Information API's endpoints are protected by the OAuth 2.0 Authorization Framework: <a href="https://tools.ietf.org/html/rfc6749" target="_blank">https://tools.ietf.org/html/rfc6749</a>

### Supported Grant Type(s)

* client_credentials

### Supported Scope(s)

* accounts

### Enrolment

In order to obtain you OAuth credentials, simply: 

1. Login to <a href="https://login.secure.investec.com" target="_blank">Investec Online.</a>
2. Navigate to the **Programmable Banking** landing page.
3. Select the **Open API** tab.
4. Select the **Enroll** button.

### Get Access Token

> Example Request

```http

POST https://openapi.investec.com/identity/v2/oauth2/token HTTP/1.1
Authorization: Basic e2NsaWVudElkfTp7c2VjcmV0fQ==
Host: openapi.investec.com
Content-Type: application/x-www-form-urlencoded
Accept: application/json

```

```javascript

$.ajax({
   url: 'https://openapi.investec.com/identity/v2/oauth2/token',
   type: 'POST',
   contentType: 'application/x-www-form-urlencoded',
   data: 'grant_type=client_credentials&scope=accounts',
   headers: {
      'Authorization': 'Basic e2NsaWVudElkfTp7c2VjcmV0fQ=='
   },
   success: function (result) {
       console.log(JSON.stringify(result));
   }
});

```

> Example Response

``` json

{
    "access_token": "Ms9OsZkyrhBZd5yQJgfEtiDy4t2c",
    "token_type": "Bearer",
    "expires_in": 1799,
    "scope": "accounts"
}

```

`POST /identity/v2/oauth2/token`

Obtain an access token

## Accounts Endpoints

### Get Accounts

> Example Request

```http

GET https://openapi.investec.com/za/pb/v1/accounts HTTP/1.1
Authorization: Bearer Ms9OsZkyrhBZd5yQJgfEtiDy4t2c
Host: openapi.investec.com
Accept: application/json

```

```javascript

$.ajax({
   url: 'https://openapi.investec.com/za/pb/v1/accounts',
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
        "accounts": [
            {
                "accountId": "172878438321553632224",
                "accountNumber": "10010206147",
                "accountName": "Mr John Doe",
                "referenceName": "My Investec Private Bank Account",
                "productName": "Private Bank Account"
            }
        ]
    },
    "links": {
        "self": "https://openapi.investec.com/za/pb/v1/accounts"
    },
    "meta": {
        "totalPages": 1
    }
}

```

`GET /za/pb/v1/accounts`

Obtain a list of accounts.

### Get Account Transactions

> Example Request

```http

GET https://openapi.investec.com/za/pb/v1/accounts/{accountId}/transactions?fromDate={fromDate}&toDate={toDate}&transactionType={transactionType} HTTP/1.1
Authorization: Bearer Ms9OsZkyrhBZd5yQJgfEtiDy4t2c
Host: openapi.investec.com
Accept: application/json

```

```javascript

$.ajax({
   url: 'https://openapi.investec.com/za/pb/v1/accounts/{accountId}/transactions?fromDate={fromDate}&toDate={toDate}&transactionType={transactionType}',
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
        "transactions": [
            {
                "accountId": "172878438321553632224",
                "type": "DEBIT",
                "transactionType": "FeesAndInterest",
                "status": "POSTED",
                "description": "MONTHLY SERVICE CHARGE",
                "cardNumber": "",
                "postedOrder": 13379,
                "postingDate": "2020-06-11",
                "valueDate": "2020-06-10",
                "actionDate": "2020-11-10",
                "transactionDate": "2020-06-10",
                "amount": 535.0,
                "runningBalance": 28857.76
            },
            {
                "accountId": "172878438321553632224",
                "type": "CREDIT",
                "transactionType": "FeesAndInterest",
                "status": "POSTED",
                "description": "CREDIT INTEREST",
                "cardNumber": "",
                "postedOrder": 13378,
                "postingDate": "2020-06-11",
                "valueDate": "2020-06-10",
                "actionDate": "2020-11-10",
                "transactionDate": "2020-06-10",
                "amount": 31.09,
                "runningBalance": 29392.76
            }
        ]
    },
    "links": {
        "self": "https://openapi.investec.com/za/pb/v1/accounts/{accountId}/transactions?fromDate={fromDate}&toDate={toDate}&transactionType={transactionType}"
    },
    "meta": {
        "totalPages": 1
    }
}

```

`GET /za/pb/v1/accounts{accountId}/transactions?fromDate={fromDate}&toDate={toDate}&transactionType={transactionType}`

Obtain a specified account's transactions.

#### Parameters

Name | In | Type | Required | Description
-|-|-|-|-
fromDate | query | ISO8601 formatted date string | optional | Refers to the date range filter's start date. Will default to today's date, minus 180 days, if not specified.
toDate | query | ISO8601 formatted date string | optional | Refers to the date range filter's end date. Will default to today's date if not specified.
transactionType | query | string | optional | Refers to the transaction type filter's value.

### Get Account Balance

> Example Request

```http
GET https://openapi.investec.com/za/pb/v1/accounts/{accountId}/balance HTTP/1.1
Authorization: Bearer Ms9OsZkyrhBZd5yQJgfEtiDy4t2c
Host: openapi.investec.com
Accept: application/json
```

```javascript

$.ajax({
   url: 'https://openapi.investec.com/za/pb/v1/accounts/{accountId}/balance',
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
        "accountId": "172878438321553632224",
        "currentBalance": 28857.76,
        "availableBalance": 98857.76,
        "currency": "ZAR"
    },
    "links": {
        "self": "https://openapi.investec.com/za/pb/v1/accounts/{accountId}/balance"
    },
    "meta": {
        "totalPages": 1
    }

```

`GET /za/pb/v1/accounts{accountId}/balance`

Obtain a specified account's balance.


## Programmable Banking Cards Endpoints  

### Get Cards
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

``` json
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
                "AccountId": "172878438321553632224",
            },
            {
                "CardKey": "1020202",
                "CardNumber": "402167XXXXXX2222",
                "IsProgrammable": true,
                "Status": "Active",
                "CardTypeCode": "VGC",
                "AccountNumber": "10011001100",
                "AccountId": "172878438321553632224",
            },
            {
                "CardKey": "1030303",
                "CardNumber": "402167XXXXXX3333",
                "IsProgrammable": true,
                "Status": "Active",
                "CardTypeCode": "VGC",
                "AccountNumber": "10011001100",
                "AccountId": "172878438321553632224",
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

## Get Function (saved) code

`GET /za/pb/v1/cards/{card_id}/code`

Obtain code currently saved to the specific card.

>Example Request:

```http

GET https://openapi.investec.com/za/pb/v1/cards/{card_id}/code HTTP/1.1
Authorization: Bearer Ms9OsZkyrhBZd5yQJgfEtiDy4t2c
Host: openapi.investec.com
Accept: application/json

```

```javascript

$.ajax({
   url: 'https://openapi.investec.com/za/pb/v1/cards/{card_id}/code',
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


## Get Function (published) code

<a name="get-function-code"></a>
`GET /za/pb/v1/cards/{card_id}/publishedcode`

Obtain code currently published to the specific card.

>Example Request:

```http

GET https://openapi.investec.com/za/pb/v1/cards/{card_id}/publishedcode HTTP/1.1
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

## Update Function Code (Without Publishing to card)

`POST /za/pb/v1/cards/{card_id}/code`

Save specified code to the specific card.  
*Note: This allows you to save/stage the code to the card without publishing it. This implies that the code will not execute when a card transaction occurs.*
>Example Request:

```http
POST https://openapi.investec.com/za/pb/v1/cards/{card_id}/code HTTP/1.1
Authorization: Bearer Ms9OsZkyrhBZd5yQJgfEtiDy4t2c
Host: openapi.investec.com
Accept: application/json

{
    "code": "// This function runs during the card transaction authorization flow.\n// It has a limited execution time, so keep any code short-running.\nconst beforeTransaction = async (authorization) => {\n    console.log(authorization);\n    return true;\n};\n\n// This function runs after a transaction.\nconst afterTransaction = async (transaction) => {\n    console.log(transaction)\n};\n\n// This function runs after a transaction.\nconst afterDecline = async (transaction) => {\n    console.log(transaction)\n};"
}
```

```javascript
$.ajax({
   url: 'https://openapi.investec.com/za/pb/v1/cards/{card_id}/code',
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
code | body | string | required | The javascript code that will be published to the card.

## Publish Function Code

`POST /za/pb/v1/cards/{card_id}/publish`

Publish specified code to the specific card.  
*Note: This allows you to push code to the specified card. After successfully publishing the code it will execute the next time a card transaction occurs.*
>Example Request:

```http
POST https://openapi.investec.com/za/pb/v1/cards/{card_id}/publish HTTP/1.1
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
   url: 'https://openapi.investec.com/za/pb/v1/cards/{card_id}/publish',
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
codeid | body | string | required | Refers to the codeid of the published code which can be obtained from the [Get Published Code](#get-function-code) endpoint.
code | body | string | required | The javascript code that will be published to the card.

## Execute Function Code (Simulation)

`POST /za/pb/v1/cards/{card_id}/code/execute`

Publish specified code to the specific card.  
*Note: This allows you to push code to the specified card. After successfully publishing the code it will execute the next time a card transaction occurs.*
>Example Request:

```http
POST https://openapi.investec.com/za/pb/v1/cards/{card_id}/publish HTTP/1.1
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
   url: 'https://openapi.investec.com/za/pb/v1/cards/{card_id}/publish',
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
simulationcode | body | string | required | The javascript code that will be published to the card.

# HTTP Status Codes

HTTP Status | Description
-|-
200 - OK | The requested operation was successfully completed
400 - Bad Request | The requested operation will not be carried out
401 - Unauthorized | The requested operation was refused access
500 - Internal Server Error | The requested operation failed to execute
