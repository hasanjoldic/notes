# `Cache-Control`

The `Cache-Control` HTTP header field holds directives — in both requests and responses — that control caching in browsers and shared caches (e.g. Proxies, CDNs).

## Vocabulary

### Shared cache

    Cache that exists between the origin server and clients (e.g. Proxy, CDN). It stores a single response and reuses it with multiple users — so developers should avoid storing personalized contents to be cached in the shared cache.

### Private cache

    Cache that exists in the client. It is also called local cache or browser cache. It can store and reuse personalized content for a single user.

### Revalidate response

    Ask the origin server whether or not the stored response is still fresh.

### Fresh response

    Indicates that the response is fresh. This usually means the response can be reused for subsequent requests.

### Stale response

    Indicates that the response is a stale response. This usually means the response can't be reused as-is.

### Age

    The time since a response was generated. It is a criterion for whether a response is fresh or stale.

## Directives

| Request        | Response               |
| -------------- | ---------------------- |
| max-age       | max-age               |
| no-cache       | no-cache               |
| no-store       | no-store               |
| -              | must-revalidate        |
| -              | private                |
| -              | stale-while-revalidate |

## Response Directives

### `max-age`

The `max-age=N` response directive indicates that the response remains fresh until N seconds after the response is generated.

### `no-cache`

The `no-cache` response directive indicates that the response can be stored in caches, but the response must be validated with the origin server before each reuse.

> Note that `no-cache` does not mean "don't cache". `no-cache` allows caches to store a response but requires them to revalidate it before reuse. If the sense of "don't cache" that you want is actually "don't store", then `no-store` is the directive to use.

### `must-revalidate`

The `must-revalidate` response directive indicates that the response can be stored in caches and can be reused while fresh. If the response becomes stale, it must be validated with the origin server before reuse.

Typically, `must-revalidate` is used with `max-age`.

HTTP allows caches to reuse stale responses when they are disconnected from the origin server. must-revalidate is a way to prevent this from happening - either the stored response is revalidated with the origin server or a 504 (Gateway Timeout) response is generated.

### `no-store`

The `no-store` response directive indicates that any caches of any kind (private or shared) should not store this response.

### `private`

The `private` response directive indicates that the response can be stored only in a private cache (e.g. local caches in browsers).

> You should add the private directive for user-personalized content, especially for responses received after login and for sessions managed via cookies.
> If you forget to add private to a response with personalized content, then that response can be stored in a shared cache and end up being reused for multiple users, which can cause personal information to leak.

### `stale-while-revalidate`

The `stale-while-revalidate` response directive indicates that the cache could reuse a stale response while it revalidates it to a cache.

## Request Directives

### `no-cache`

The `no-cache` request directive asks caches to validate the response with the origin server before reuse.

### `no-store`

The `no-store` request directive allows a client to request that caches refrain from storing the request and corresponding response — even if the origin server's response could be stored.

### `max-age`

The `max-age=N` request directive indicates that the client allows a stored response that is generated on the origin server within N seconds — where N may be any non-negative integer (including 0).