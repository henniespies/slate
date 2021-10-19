# Open API - Accounts

The Account Information API allows Investec SA Private Banking clients to access their account and transactional information (read-only) via an API.

It can be used to retrieve account information such as:

* Account details
* Balances
* Account transactions

There are many possible use cases for the Account Information API: from extracting the data to aggregate it with financial data from other banking institutions to personal money management tools.

### Release Notes (Accounts)

#### 2020-11-13

* **Enhancement**: Included transaction type filter to <a href="#get-account-transactions">Get Account Transactions</a>

#### 2020-11-10

* **Enhancement**: Included postedOrder to <a href="#get-account-transactions">Get Account Transactions</a>
* **Enhancement**: Included transactionType to <a href="#get-account-transactions">Get Account Transactions</a>

#### 2020-09-08

* **Fix**: Implemented CORS support
* **Fix**: Implemented multi User-Agent support

#### 2020-07-21

* **Enhancement**: Included transactionDate to <a href="#get-account-transactions">Get Account Transactions</a>
* **Enhancement**: Included date range filter to <a href="#get-account-transactions">Get Account Transactions</a>

#### 2021-10-01

<hr>

## Get Accounts

`GET /za/pb/v1/accounts`

Obtain a list of accounts in associated profile.

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
## Get Account Balance

`GET /za/pb/v1/accounts{accountId}/balance`

Obtain a specified account's balance.

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

## Get Account Transactions

`GET /za/pb/v1/accounts{accountId}/transactions?fromDate={fromDate}&toDate={toDate}&transactionType={transactionType}`

Obtain a specified account's transactions.

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

### Parameters

Name | In | Type | Required | Description
-|-|-|-|-
fromDate | query | ISO8601 formatted date string | optional | Refers to the date range filter's start date. Will default to today's date, minus 180 days, if not specified.
toDate | query | ISO8601 formatted date string | optional | Refers to the date range filter's end date. Will default to today's date if not specified.
transactionType | query | string | optional | Refers to the transaction type filter's value.


## HTTP Status Codes

HTTP Status | Description
-|-
200 - OK | The requested operation was successfully completed
400 - Bad Request | The requested operation will not be carried out
401 - Unauthorized | The requested operation was refused access
429 - Too Many Requests | To many requests in quick succession, Retry the request.
500 - Internal Server Error | The requested operation failed to execute
