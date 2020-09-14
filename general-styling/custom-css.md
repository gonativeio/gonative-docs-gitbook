# Custom CSS

You may enter any custom CSS that you would like applied to each page in your app.

One of the most common reasons is to hide your web navigation elements when using native sidebar navigation. You may also hide arbitrary web elements within your app, such as the header and footer.

```css
.nav
{ 
	display: none !important; 
}

#header, #footer
{ 
	display: none !important; 
}
```

### Server-side option \(advanced\)

You may also apply custom CSS rules through your server by detecting the request coming from GoNative, and then serving different or additional CSS resources. More info about this technique at [https://support.gonative.io/help/how-do-i-detect-usage-coming-from-my-apps](https://support.gonative.io/help/how-do-i-detect-usage-coming-from-my-apps).

### Learn more

* [CSS Tutorials by HTML Dog](http://www.htmldog.com/guides/css/)

