# NativeJS - Status bar styling

Dynamically customize status bar visibility and style

To customize the status bar, open the url:

`gonative://statusbar/set` via a link, or via javascript using `window.location.href = 'gonative://statusbar/set';`

with query params \(all optional\):

* - style: "light" or "dark". Sets the icon colors in the status bar. Default on iOS is dark and default on Android is light. Setting Android to dark icons requires Marshmallow 6.0 or later.
* - color: in RRBBGG or AARRBBGG format with hex values. Sets the status bar to a solid color. On Android, requires Lollipop 5.0 or later. Use 00000000 for completely transparent.
* - overlay: "true" or "false". If true, web content will extend underneath the status bar. If false \(default behavior\), web content will start below the bottom of the status bar.

For example, to set a red status bar with 50% alpha, white icons, and have the web content extend underneath the status bar:

`window.location.href = 'gonative://statusbar/set?style=light&color=80ff0000&overlay=true';`

