# Scandit Barcode Scanner

_Once the premium module has been added to your app, you may use the following APIs to access its functionality._

### Scandit Barcode Scanner

Once the premium module has been added to your app, you may use the following APIs to access its functionality.

To scan a barcode, define a function in javascript, for example:

```javascript
function scandit_result(data) { 
    console.log('scanned barcode ' + data.code + ' of type ' + data.type); 
}
```

Initiate a scan by opening `gonative://scandit-barcode/scan?callback=scandit_result`

By default, we scan for these symbologies: 

* ean13
* upc12
* ean8
* upce
* code39
* code128
* itf
* qr
* datamatrix
* pdf417

If you would like to limit the list, you can add a query parameter with a comma-separated list of symbologies. For example, to only scan qr and pdf417 codes:

Initiate a scan by opening: 

`gonative://scandit-barcode/scan?callback=scandit_result&symbologies=qr,pdf417`

For some symbologies such as pdf417, you will need to specify a parser to use in parsing the data. For example, you will need to add an extra query parameter, parser=dlid to your gonative://scandit-barcode call. 

For example:

`gonative://scandit-barcode/scan?callback=barcode_callback&symbologies=qr,code128&parser=dlid`

