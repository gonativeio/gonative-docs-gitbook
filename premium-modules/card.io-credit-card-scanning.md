# Card.IO credit card scanning

_Once the premium module has been added to your app, you may use the following APIs to access its functionality_.

### Overview

This module integrates the Card.IO Native SDK into your app. More info at [https://www.card.io/](https://www.card.io/)

### Implementation Guide

Once the premium module has been added to your app, you may use the following APIs to access its functionality.

### Scanning a card

To scan a credit card, define a function in javascript, for example:

```javascript
function processCard(data) {
  alert(JSON.stringify(data));
}

```

Initiate a scan by opening `gonative://card.io/scanCard&callbackFunction=processCard`

You can do this via an "a href" link, or via javascript:

`window.location.href= ‘gonative://card.io/scanCard&callbackFunction=processCard';`

### Scanning options

Optional query params include:

- requireExpiry: default true. Get the expiry date.

- scanExpiry: default true. Attempt to automatically determine the expiry date.

- requireCVV: default true. Get the CVV of the card.

- requirePostalCode: default false. Get the postal code of the cardholder.

- numericPostalCode: default false. Set to true if you know the postal code must be numeric.

- requireCardholderName: default false. Get the cardholder's name.

- useCardIOLogo: default false. Show card.io logo instead of Paypal logo. Obviated if hideCardIOLogo is true.

- hideCardIOLogo: default true. Hide the logo from the UI.

- instructions: a custom instruction string to show on the UI. Card.io provides a default string.

For example:

```javascript
window.location.href = "gonative://card.io/scanCard?callbackFunction=processCard&requirePostalCode=true&numericPostalCode=true";
```

### Scanning result

On a successful card scan, the callback function will be called with a javascript object as it's parameter. The object may have the following fields:

- cardNumber: \(string\)

- cardholderName: \(string\)

- expiryMonth: \(number\)

- expiryYear: \(number\)

- cardType: \(string\) "visa", "mastercard", "amex", "discover", or "jcb"

