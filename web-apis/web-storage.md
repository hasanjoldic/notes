# Web Storage API

The Web Storage API provides mechanisms by which browsers can store key/value pairs, in a much more intuitive fashion than using cookies.

The two mechanisms within Web Storage are:

- `sessionStorage` maintains a separate storage area for each given origin that's available for the duration of the page session.

  - Stores data only for a session, meaning that the data is stored until the browser (or tab) is closed.
  - Data is never transferred to the server.
  - Storage limit is larger than a cookie (at most 5MB).

- `localStorage` does the same thing, but persists even when the browser is closed and reopened.

The keys and the values are always strings (note that, as with objects, integer keys will be automatically converted to strings). You can access these values like an object, or with the `Storage.getItem()` and `Storage.setItem()` methods. These three lines all set the (same) colorSetting entry:

```js
localStorage.colorSetting = "#a4509b";
localStorage["colorSetting"] = "#a4509b";
localStorage.setItem("colorSetting", "#a4509b");
```

