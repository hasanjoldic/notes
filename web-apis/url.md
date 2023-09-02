# URL API

The URL API is a component of the URL standard, which defines what constitutes a valid __Uniform Resource Locator__ and the API that accesses and manipulates URLs. The URL standard also defines concepts such as __domains__, __hosts__, and __IP addresses__, and also attempts to describe in a standard way the legacy `application/x-www-form-urlencoded` MIME type used to submit web forms' contents as a set of key/value pairs.

```js
let addr = new URL("https://developer.mozilla.org/en-US/docs/Web/API/URL_API");
let host = addr.host;
let path = addr.pathname;
```

Most of the properties of URL are settable; you can write new values to them to alter the URL represented by the object:

```js
let myUsername = "someguy";
let addr = new URL("https://example.com/login");
addr.username = myUsername;
```

__Setting the value of username not only sets that property's value, but it updates the overall URL.__

The search property on a URL contains the query string portion of the URL. For example, if the URL is `https://example.com/login?user=someguy&page=news,` then the value of the search property is `?user=someguy&page=news`.

```js
let addr = new URL("https://example.com/login?user=someguy&page=news");
try {
  loginUser(addr.searchParams.get("user"));
  gotoPage(addr.searchParams.get("page"));
} catch (err) {
  showErrorMessage(err);
}
```

