# Open Source Scanner

### Overview

This implementation of Barcode Scanning SDK uses open source libraries which do not require any additional paid license.

For Android, we are using Zebra Crossing library at [https://github.com/zxing/zxing](https://github.com/zxing/zxing)  
 For iOS, we are using hyperoslo library at [https://github.com/hyperoslo/BarcodeScanner](https://github.com/hyperoslo/BarcodeScanner)

### Implementation Guide

Once the premium module has been added to your app, you may use the following APIs to access its functionality.

To scan a barcode, define a function in javascript, for example:

```javascript
function process_barcode(data) { if (data.success) {
    alert('got barcode', data.code); }
}
```

Initiate a scan by opening `gonative://barcode/scan?callback=process_barcode`

You can do this via an "a href" link, or via javascript:

`window.location.href='gonative://barcode/scan?callback=process_barcode';`

