# Access to native contacts

_Once the premium module has been added to your app, you may use the following APIs to access its functionality._

### Implementation Guide

We have added a javascript native bridge to allow retrieval of user contacts.

To use it, you will need to create a javascript function on your web page:

```javascript
function contacts_callback(data) { 
    // process the data here. In this example, we are just logging it 
    alert(JSON.stringify(data)); 
}
```

Then open this url, passing in the name of your callback function:

`window.location.href = 'gonative://contacts/getAll?callback=contacts_callback';`

The app will prompt the user for permission. If granted, it will call contacts\_callback with this object as data: 

```javascript
{ 
    success: true, 
    contacts: [...] 
}
```

"contacts" will be an array of contact objects. Each object may have the following fields:

iOS: 

* birthday
* namePrefix
* givenName
* middleName
* familyName
* previousFamilyName
* nameSuffix
* nickname
* phoneticGivenName
* phoneticMiddleName
* phoneticFamilyName
* organizationName
* departmentName
* jobTitle
* note
* phoneNumbers
* emailAddresses
* postalAddresses  

Android: 

* birthday
* givenName
* familyName
* companyName
* companyTitle
* note
* phoneNumbers
* emailAddresses
* postalAddresses

phoneNumbers, emailAddresses, and postalAddresses are arrays of objects, described below. All other fields are strings.

Each phoneNumber is an object with fields: label phoneNumber

Each emailAddress is an object with fields: 

* label
* emailAddress

Each postalAddress is an object with fields: 

* label
* street
* city
* state - iOS only
* region - Android only
* postalCode
* country
* isoCountryCode - iOS only
* subAdministrativeArea - iOS only
* subLocality - iOS only

### Checking Permissions 

The app will automatically prompt users for permissions as necessary. To separately check if permission have been granted, open the url:

`window.location.href = 'gonative://contacts/getPermissionStatus?callback=CALLBACK';`

The CALLBACK function will be run with an object like: `{"status": "granted"}`

The possible statuses on iOS are: 

* granted – user has granted permission
* denied – user has explicitly denied permission
* restricted – access has been administratively prohibited
* notDetermined – user has not yet been asked for permission

The possible statuses on Android are: 

* granted – user has granted permission 
* denied – user has not yet been asked, or has explicitly denied permission

