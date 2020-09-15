# Bluetooth beacons

_Once the premium module has been added to your app, you may use the following APIs to access its functionality_.

## Overview

This module allows the scanning of nearby Bluetooth Low-Energy beacons that conform to the iBeacon spec. It provides a native bridge to retrieve the list of nearby beacons and their identifying information.

An iBeacon continuously broadcasts three identifiers: a UUID, major, and minor. UUID is a long random identifier that should be customized for each implementation. Major and minor are integers between 0 and 65535 that can be used to differentiate each individual beacon. For example, a retailer may have one UUID across all of their iBeacon deployments used for their retail customers, and another UUID used for employees. Each store may have a major identifier, and each individual beacon would have a different minor identifier.

The beacon broadcast data also encodes the emitting signal power. By comparing this to the actual received signal power, devices can estimate their distance from each beacon.

As Bluetooth beacons can be used to identify the user's physical location, this is considered a privacy sensitive function. iOS requires the user to grant location permissions. Android also requires the user to grant permissions starting with Android 10.

## Implementation Guide

You must provide a specific UUID to scan for iBeacons. Apple does not permit iOS devices to scan for all UUIDs.

Create a function that will receive the beacon data. Then open the url `gonative://beacon/scan?callback=callback&uuid=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`

For example:

```javascript
// define this function, but do not call it yourself!
function callback(data) {
  if (data.success) {
    alert('I found ' + data.beacons.length + ' beacons');
  }
}

window.location.href = "gonative://beacon/scan?callback=callback&uuid=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";

```

Within a few seconds of a successful scan, the app will run your callback function with a data object with the fields:

* success: set to true. This does not indicate that any beacons were found, only that the scan did not encounter errors.
* beacons: an array of beacon objects, each containing the fields:
  * format: a string, set to "iBeacon"
  * uuid: a string
  * major: an integer
  * minor: an integer
  * rssi: signal strength in decibels
  * proximity \(iOS only\): "immediate", "near", "far", or "unknown"
  * accuracy \(iOS only\): the accuracy of the proximity calculation in meters
  * distance: \(Android only\): the estimated distance of the beacon in meters

For example:

```javascript
{
  "success": true,
  "beacons": [
{
  "format": "iBeacon",
  "uuid": "9C615AD4-8E6A-53BC-BC27-82171491A2WA",
  "major": 1000,
  "minor": 123,
  "rssi": -52,
  "proximity": "immediate",
  "accuracy": 0.1413
},
{
  "format": "iBeacon",
  "uuid": "9C615AD4-8E6A-53BC-BC27-82171491A2WB",
  "major": 1001,
  "minor": 1321,
  "rssi": -78,
  "proximity": "near",
  "accuracy": 4.5321
}
  ]
}

```

If the scan fails, the data object will have the fields:

* success: set to false
* error: a string that describes why the scan failed

For example:

```javascript
{
  "success": false,
  "error": "This device does not support Bluetooth LE"
}

```

Typical reasons for a scan to fail include the device not having the necessary hardware, Bluetooth being disabled, or the user denying location permissions.

