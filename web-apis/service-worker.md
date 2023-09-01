# Service worker API

[MDN - Service worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
[MDN - Using service workers](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers)

Service workers essentially act as proxy servers that sit between web applications, the browser, and the network (when available). They are used for:

- Background data synchronization.
- Responding to resource requests from other origins.
- Receiving centralized updates to expensive-to-calculate data such as geolocation or gyroscope, so multiple pages can make use of one set of data.
- Hooks for background services.
- Performance enhancements, for example pre-fetching resources that the user is likely to need in the near future, such as the next few pictures in a photo album.
- API mocking.

A service worker is an event-driven worker registered against an __origin and a path__. It takes the form of a JavaScript file that can control the web-page/site that it is associated with.

A service worker is run in a worker context: __it therefore has no DOM access, and runs on a different thread to the main JavaScript__, so it is non-blocking. It is designed to be fully async; as a consequence, APIs such as __Web Storage__ can't be used inside a service worker.

Service workers can't import JavaScript module dynamically, and `import()` will throw if it is called in a service worker global scope. Static import using the `import` statement is allowed.

Service workers only run over HTTPS, for security reasons.

In __Firefox__, service worker APIs are also hidden and cannot be used when the user is in private browsing mode.

> Note: On Firefox, for testing you can run service workers over HTTP (insecurely); simply check the Enable Service Workers over HTTP (when toolbox is open) option in the Firefox Devtools options/gear menu.

## Download, install and activate

Service workers observe the following lifecycle:

- Download
- Install
- Activate

## Basic architecture

1. The service worker code is __fetched__ and then registered using `serviceWorkerContainer.register()`. If successful, the service worker is executed in a `ServiceWorkerGlobalScope`; this is basically a special kind of worker context, running off the main script execution thread, __with no DOM access__. The service worker is now ready to process events.

2. __Installation__ takes place. An `install` event is always the first one sent to a service worker.

3. When the `install` handler completes, the service worker is considered installed. At this point a previous version of the service worker may be active and controlling open pages. Because we don't want two different versions of the same service worker running at the same time, __the new version is not yet active__.

4. Once all pages controlled by the old version of the service worker have closed, it's safe to retire the old version, and the newly installed service worker receives an `activate` event. The primary use of `activate` is to clean up resources used in previous versions of the service worker. The new service worker can call `skipWaiting()` to ask to be activated immediately without waiting for open pages to be closed. The new service worker will then receive `activate` immediately, and will take over any open pages.

5. After activation, the service worker will now control pages, __but only those that were opened after the `register()` is successful__. In other words, documents will have to be reloaded to actually be controlled, __because a document starts life with or without a service worker and maintains that for its lifetime__. To override this default behavior and adopt open pages, a service worker can call `clients.claim()`.

6. Whenever a new version of a service worker is fetched, this cycle happens again and the remains of the previous version are cleaned during the new version's activation.

## Events

- install
- activate
- message
- fetch
- sync
- push

## Registering

```js
const registerServiceWorker = async () => {
  if ("serviceWorker" in navigator) {
    try {
      const registration = await navigator.serviceWorker.register("/sw.js", {
        scope: "/",
      });
      if (registration.installing) {
        console.log("Service worker installing");
      } else if (registration.waiting) {
        console.log("Service worker installed");
      } else if (registration.active) {
        console.log("Service worker active");
      }
    } catch (error) {
      console.error(`Registration failed with ${error}`);
    }
  }
};

// …

registerServiceWorker();
```

A single service worker can control many pages. Each time a page within your scope is loaded, the service worker is installed against that page and operates on it. Bear in mind therefore that you need to be careful with global variables in the service worker script: __each page doesn't get its own unique worker__.

## Install and activate

The `install` event is fired when an installation is successfully completed. The `install` event is generally used to populate your browser's offline caching capabilities with the assets you need to run your app offline. To do this, we use Service Worker's storage API — `caches` — a global object on the service worker that allows us to store assets delivered by responses, and keyed by their requests. This API works in a similar way to the browser's standard cache, but it is specific to your domain. The contents of the cache are kept until you clear them.

```js
const addResourcesToCache = async (resources) => {
  const cache = await caches.open("v1");
  await cache.addAll(resources);
};

self.addEventListener("install", (event) => {
  event.waitUntil(
    addResourcesToCache([
      "/",
      "/index.html",
      "/style.css",
      "/app.js",
      "/image-list.js",
      "/star-wars-logo.jpg",
      "/gallery/bountyHunters.jpg",
      "/gallery/myLittleVader.jpg",
      "/gallery/snowTroopers.jpg",
    ]),
  );
});
```

## Custom responses to requests

```js
const putInCache = async (request, response) => {
  const cache = await caches.open("v1");
  await cache.put(request, response);
};

const cacheFirst = async ({ request, fallbackUrl }) => {
  // First try to get the resource from the cache
  const responseFromCache = await caches.match(request);
  if (responseFromCache) {
    return responseFromCache;
  }

  // Next try to get the resource from the network
  try {
    const responseFromNetwork = await fetch(request);
    // response may be used only once
    // we need to save clone to put one copy in cache
    // and serve second one
    putInCache(request, responseFromNetwork.clone());
    return responseFromNetwork;
  } catch (error) {
    const fallbackResponse = await caches.match(fallbackUrl);
    if (fallbackResponse) {
      return fallbackResponse;
    }
    // when even the fallback response is not available,
    // there is nothing we can do, but we must always
    // return a Response object
    return new Response("Network error happened", {
      status: 408,
      headers: { "Content-Type": "text/plain" },
    });
  }
};

self.addEventListener("fetch", (event) => {
  event.respondWith(
    cacheFirst({
      request: event.request,
      fallbackUrl: "/gallery/myLittleVader.jpg",
    }),
  );
});

```

`caches.match(event.request)` allows us to match each resource requested from the network with the equivalent resource available in the cache, if there is a matching one available. The matching is done via URL and various headers, just like with normal HTTP requests.

If the resources aren't in the cache, they are requested from the network.

Cloning the response is necessary because request and response streams can only be read once. In order to return the response to the browser and put it in the cache we have to clone it. So the original gets returned to the browser and the clone gets sent to the cache. They are each read once.

