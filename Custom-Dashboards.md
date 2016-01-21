# Custom Dashboards

You can:

- create your own dashboards using simple HTML (no javascript is required for basic dashboards)
- utilizing any or all of the available chart libraries, on the same dashboard
- using data from one or more netdata servers, on the same dashboard
- host your dashboard HTML page on any web server, anywhere

netdata charts can also be added to existing web pages.

## Example empty dashboard

If you need to create a new dashboard on an empty page, we suggest the following header:

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<title>Your dashboard</title>

	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<meta charset="utf-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
	<meta name="viewport" content="width=device-width, initial-scale=1">
	<meta name="apple-mobile-web-app-capable" content="yes">
	<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">

	<!-- here we will add dashboard.js -->

</head>
<body>

<!-- here we will add charts -->

</body>
</html>
```


## dashboard.js

To add netdata charts to any web page (dedicated to netdata or not), you need to include the `/dashboard.js` file of a netdata server.

For example, if your netdata server listens at `http://box:19999/`, you will need to add the following to the `head` section of your web page:

```html
  <script type="text/javascript" src="http://box:19999/dashboard.js"></script>
```

### what dashboard.js does?

`dashboard.js` will automatically load the following:

1. `dashboard.css`, required for the netdata charts

2. `jquery.min.js`, if not already loaded

3. `bootstrap.min.js` (only of bootstrap is not already present) and `bootstrap.min.css`.

   You can disable this by adding the following before loading `dashboard.js`:

   ```html
 <script>var netdataNoBootstrap = true;</script>
   ```

4. `jquery.nanoscroller.min.js`, required for the scrollbar of the chart legends.

5. `bootstrap-toggle.min.js` and `bootstrap-toggle.min.css`, required for the settings toggle buttons.

6. `font-awesome.min.css`, for icons.

When `dashboard.js` loads will scan the page for elements that define charts (see below) and immediately start refreshing them.

### Prevent dashboard.js from starting chart refreshes

If your web page is not static and you plan to add charts using javascript, you can tell `dashboard.js` not to start processing charts immediately after loaded, by adding this fragment before loading it:

```html
<script>var netdataDontStart = true;</script>
```

The above, will inform the `dashboard.js` to load everything, but not process the web page until you tell it to.
You can tell it to start processing the page, by running this javascript code:

```js
NETDATA.start();
```

Be careful not to call the `NETDATA.start()` multiple times. Each call to this function will spawn a new thread that will start refreshing the charts.

If, after calling `NETDATA.start()` you need to update the page (or even get your javascript code synchronized with `dashboard.js`), you can call (after you loaded `dashboard.js`):

```js
NETDATA.pause(function() {
  // ok, it is paused

  // update the DOM as you wish

  // and then call this to let the charts refresh:
  NETDATA.unpause();
});
```

### The default netdata server

`dashboard.js` will attempt to auto-detect the URL of the netdata server it is loaded from, and set this server as the default netdata server for all charts.

If you need to set any other URL as the default netdata server for all charts that do not specify a netdata server, add this before loading `dashboard.js`:

```html
<script type="text/javascript">var netdataServer = "http://your.netdata.server:19999";</script>
```

## Adding charts
