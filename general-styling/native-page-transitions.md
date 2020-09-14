# Native page transitions

With native page transitions enabled, when a user navigates to a new URL within your app, the page will fade out, show a loading spinner, and then fade in once the new page is ready.

Specifically, the transition will start when a new non-AJAX HTTP request is made, and will fade in once the DOM readyState is "interactive". More info at [readyState documentation at Mozilla](https://developer.mozilla.org/en-US/docs/Web/API/Document/readyState).

Note: If your app feels slower than your mobile website, please first disable native page transitions, rebuild, and verify that it is now the same speed.

If this is the case, it's very likely your web application has long-running javascript before firing the readyState interactive event. Javascript that is not required before rendering the page should be run after `$( document ).ready()` or similar so that it does not hold up the loading of your website, and so the native page transitions can finish.

