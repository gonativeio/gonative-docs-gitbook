# Document Scanning SDK

### Implementation Guide

Once the premium module has been added to your app, you may use the following APIs to access its functionality.

Create a javascript function to accept a scanned image. It will be called with an object with the fields:

* `image`: a base64 encoded string representing the jpeg image file
* `mimeType`: 'image/jpeg'
* `encoding`: 'base64'

To initiate a scan, open the url:

`gonative://documentScanner/scanPage?callback=CALLBACKFUNCTION`

Replace CALLBACKFUNCTION with your actual javascript function name.

Here is the source code for a demo that simply displays the scanned image.

```markup
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Document Scanner Test</title>
<script>
function callback(data) {
document.getElementById('page').setAttribute('src', 'data:' + data.mimeType +
';' + data.encoding + ', ' + data.image);
}
</script>
<style>
img
{
    max-width: 100%;
    min-width: 300px;
    height: auto;
}
</style>
</head>
<body>

<h1>Document Scanner test</h1>

<p><a href="gonative://documentScanner/scanPage?callback=callback">Scan page</a></p>

<p><img id="page" src=""/></p>

</body> 
</html>

```

