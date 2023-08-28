# Resource hints

## `dns-prefetch`

Tells the browser that it should preemptively perform DNS resolution for that origin.

## `preconnect`

Tell the browser that it should preemptively perform part or all of the handshake (DNS+TCP for HTTP, and DNS+TCP+TLS for HTTPS origins).

## `prefetch`

Tell the browser that it should preemptively fetch and cache the resource.

## `preload`

The preload value of the `<link>` element's rel attribute lets you declare fetch requests in the HTML's `<head>`, specifying resources that your page will need very soon, which you want to start loading early in the page lifecycle, before browsers' main rendering machinery kicks in.

> Even though the name contains the term load, it doesn't load and execute the script but only schedules it to be downloaded and cached with a higher priority.
