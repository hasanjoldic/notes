# Storage API

> Note: This feature is available in Web Workers

> Secure context: This feature is available only in secure contexts (HTTPS), in some or all supporting browsers.

The data stored for a website which is managed by the Storage Standard usually includes **IndexedDB** databases and **Cache API** data, but may include other kind of site-accessible data such as **Web Storage API** data.

## Storage buckets

The storage system described by the Storage Standard, where site data is stored, usually consists of a single bucket for each origin.

In essence, every website has its own storage space into which its data gets placed. In some cases however, user agents may decide to store a single origin's data in multiple different buckets, for example when this origin is embedded in different third-party origins.

## What technologies store data in the browser?

| Technology                        | Description                                                                                                                                      |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| Cookies                           | An HTTP cookie is a small piece of data that the web server and browser send each other to remember stateful information across page navigation. |
| Web Storage                       | The Web Storage API provides mechanisms for webpages to store string-only key/value pairs, including `localStorage` and `sessionStorage`.        |
| IndexedDB                         | IndexedDB is a Web API for storing large data structures in the browser and indexing them for high-performance searching.                        |
| Cache API                         | The Cache API provides a persistent storage mechanism for HTTP request and response object pairs that's used to make webpages load faster.       |
| Origin Private File System (OPFS) | OPFS provides a file system that's private to the origin of the page and can be used to read and write directories and files.                    |

## Does browser-stored data persist?

Data for an origin can be stored in two ways:

1. _default_ **Best-effort** - persists as long as the origin is below its quota, the device has enough storage space, and the user doesn't choose to delete the data via their browser's settings.

2. **Persistent**: an origin can opt-in to store its data in a persistent way. Data stored this way is only evicted, or deleted, if the user chooses to, by using their browser's settings.

```js
if (navigator.storage && navigator.storage.persist) {
  navigator.storage.persist().then((persistent) => {
    if (persistent) {
      console.log("Storage will not be cleared except by explicit user action");
    } else {
      console.log("Storage may be cleared by the UA under storage pressure.");
    }
  });
}
```

## How much data can be stored?

### Cookies

A browser should be able to accept at least 300 cookies with a maximum size of 4096 bytes.

### Web Storage

Web Storage, which can be accessed by using the `localStorage` and `sessionStorage` properties of the `window` object, is limited to `10 MiB` of data maximum on all browsers.

Browsers can store up to 5 MiB of local storage, and 5 MiB of session storage per origin.

Once this limit is reached, browsers throw a `QuotaExceededError` exception.

### Other web technologies

The data that's stored by using other web technologies, such as IndexedDB, Cache API, or File System API (which defines the Origin Private File System), is managed by a storage management system that's specific to each browser.
