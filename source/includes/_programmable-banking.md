# Programmable Banking
> Example conditional transaction approval

```javascript
// Check with my API before approving transaction
const beforeTransaction = async (authorization) => {
    const response = await fetch('https://postman-echo.com/get?foo1=bar1&foo2=bar2', {
        method: 'GET',
        headers: {'Content-Type': 'application/json'}
    });
    const json = await response.json();
    // foo and bar available via json.args
    console.log(json.args);
    return json.args.foo1 === 'bar1';
};

const afterTransaction = async (transaction) => {
    // do something after the `transaction` is approved or declined
};
```
> Above code's simulated output

```json
{
    "beforeTransaction": {
        "time": null,
        "output": {
            "sandbox": true,
            "type": "before_transaction",
            "authorizationApproved": true,
            "logs": [
                {
                    "foo1": "bar1",
                    "foo2": "bar2"
                }
            ],
            "createdAt": "/Date(1592426721179)/",
            "startedAt": "/Date(1592426721179)/",
            "completedAt": "/Date(1592426722389)/",
            "updatedAt": "/Date(1592426721179)/",
            "error": null
        }
    },
    "afterTransaction": {
    ...
}
```

Build your own rules and limits with securely hosted Javascript functions that execute before and after your card transactions. Limit your fast-food spend, track your coffee intake on the fly - anything goes.

[![IMAGE ALT TEXT HERE](images/overview.gif)](https://www.youtube.com/watch?v=lO-fSgNOUkY)

**Niche developer banking, built with the community**

Investec Private Bank offers distinctive banking for distinguished individuals. Software development is a profession of the future, and Investec and OfferZen are working together with the community on a mission to enable developers to access the world of banking.

# Prerequisite

This documentation assumes that you have a basic understanding of Javascript and, are aware of the consequences of executing programmatic logic against your card transactions, e.g. resulting in a transaction being decline as a result of logic you have programmed against your Investec credit card.

## Features included

- Online JS IDE/code editor based on Monaco Editor, the code editor that powers VS Code
- Two hooks which you can access with every card transaction, `beforeTransaction` and `afterTransaction`
- Intercept card transaction and decline based on your "code" with `beforeTransaction` hook
- Publish "code" to securely hosted VM instance
- Perform transaction simulation using online simulator
- Enable/disable the code executing against a specific card
- Access to the card authorization object and meta-data
- Run code after a processed using the afterTransaction() function. for instance: post your transaction to an app you built
- Call external APIs using node-fetch

# main.js
> beforeTransaction and afterTransaction are required

```javascript
// main.js
const beforeTransaction = async () => {
    // ..
}

const afterTransaction = async () => {
    // ..
}
```

`main.js` requires two methods for card transaction interception:

- `beforeTransaction`
- `afterTransaction`

Note that the `beforeTransaction` method has a limited window of 2 seconds to execute hence needs to be fast. `afterTransaction` has a much larger window limit of 15 seconds which gives you more time to communicate with internal and external resources.

![main.js](images/main-js.png)

## Before transaction

Every time you execute a transaction from your Investec card, the `beforeTransaction` method intercepts the authorization object before it is approved by Investec.

Within this method, you can apply logic to either **decline or approve** the transaction based on data from the card authorization itself, or via external data sources fetched via http. Please note that **you MUST return a boolean in order for this method to work**.

### Approve authorization
> Approve a transaction

```javascript
// main.js
const beforeTransaction = async () => {
    // always APPROVE
    return true;
}
```
`return true` will approve an authorization.

### Decline authorization

> Decline a transaction

```javascript
// main.js
const beforeTransaction = async () => {
    // always DECLINE
    return false;
}
```
`return false` will decline a authorization.

### Apply conditional logic within before transaction method

> Apply conditional to a transaction

```javascript
// main.js
const beforeTransaction = async (authorization) => {
    return authorization.centsAmount < 5000; // authorization.amount is in cents
}
```
Apply conditional logic within before transaction method
Say you wanted to decline any transaction that is R50.00 and above - if the authorization.centsAmount is less than R50.00, then approve. Else decline the transaction.

*TIP: Use console.log(authorization) to see available properties of the authorization that are accessible.*

## After transaction

> Log transaction object to console

```javascript
// main.js
const afterTransaction = async (transaction) => {
    console.log(transaction);
}

```
The afterTransaction method is executed once the beforeTransaction method has completed and returned a response. This method has a much longer window of time for your to interact/communicate with data sources such as updating a external DB.

# env.json

> Define a environment variable in env,json

```json
{
    "foo": "bar"
}
```
Within `env.json`, you are able to save variable - a variable whose value is set outside main.js. An environment variable is made up of a name/value pair, and any number may be created and available for reference at a point in time within main.js.

To reference a value define in env.js within main.js, you can access all your variables via process.env.

![main.js](images/env-json.png)

> Access environment variable within main.js

```javascript
// main.js
const afterTransaction = async (transaction) => {
    // console log the foo key's value
    console.log(process.env.foo);
}
```
Once a environment variable has been defined and saved, you can access it from within `main.js`.

# Simulating a transaction

> use `reference` to identify simulated transactions

```javascript
// main.js
const afterTransaction = async (transaction) => {
    if(transaction.reference === 'simulation') {
        // this is a simulated transaction
    }
}

```

You can differentiate between a simulated transaction and a production transaction by the transaction reference

![main.js](images/simulate.png)
