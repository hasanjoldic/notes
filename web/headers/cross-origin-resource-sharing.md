# Cross-Origin Resource Sharing (CORS)

Cross-Origin Resource Sharing (CORS) is an HTTP-header based mechanism that allows a server to indicate any origins (domain, scheme, or port) other than its own from which a browser should permit loading resources. CORS also relies on a mechanism by which browsers make a "preflight" request to the server hosting the cross-origin resource, in order to check that the server will permit the actual request. In that preflight, the browser sends headers that indicate the HTTP method and headers that will be used in the actual request.

## `Access-Control-Request-Headers`

The `Access-Control-Request-Headers` request header is used by browsers when issuing a preflight request to let the server know which HTTP headers the client might send when the actual request is made (such as with setRequestHeader()). The complementary server-side header of `Access-Control-Allow-Headers` will answer this browser-side header.

## `Access-Control-Request-Method`

The `Access-Control-Request-Method` request header is used by browsers when issuing a preflight request, to let the server know which HTTP method will be used when the actual request is made. This header is necessary as the preflight request is always an OPTIONS and doesn't use the same method as the actual request.

## `Access-Control-Allow-Headers`

The `Access-Control-Allow-Headers` response header is used in response to a preflight request which includes the `Access-Control-Request-Headers` to indicate which HTTP headers can be used during the actual request.

### Request

First, the request. The preflight request is an `OPTIONS` request that includes some combination of the three preflight request headers: `Access-Control-Request-Method`, `Access-Control-Request-Headers`, and `Origin`.

```http
OPTIONS /resource/foo
Access-Control-Request-Method: GET
Access-Control-Request-Headers: Content-Type, x-requested-with
Origin: https://foo.bar.org
```

### Response

If the CORS request indicated by the preflight request is authorized, the server will respond to the preflight request with a message that indicates the allowed origin, methods, and headers.

```http
HTTP/1.1 200 OK
Content-Length: 0
Connection: keep-alive
Access-Control-Allow-Origin: https://foo.bar.org
Access-Control-Allow-Methods: POST, GET, OPTIONS, DELETE
Access-Control-Allow-Headers: Content-Type, x-requested-with
Access-Control-Max-Age: 86400
```

If the requested method isn't supported, the server will respond with an error.

## `Access-Control-Allow-Methods`

The `Access-Control-Allow-Methods` response header specifies one or more methods allowed when accessing a resource in response to a preflight request. 

## `Access-Control-Allow-Origin`

The `Access-Control-Allow-Origin` response header indicates whether the response can be shared with requesting code from the given origin.

## `Access-Control-Expose-Headers`

The `Access-Control-Expose-Headers` response header allows a server to indicate which response headers should be made available to scripts running in the browser, in response to a cross-origin request.

Only the CORS-safelisted response headers are exposed by default. For clients to be able to access other headers, the server must list them using the `Access-Control-Expose-Headers` header.

### CORS-safelisted response header

By default, the safelist includes the following response headers:

  `Cache-Control`
  `Content-Language`
  `Content-Length`
  `Content-Type`
  `Expires`
  `Last-Modified`
  `Pragma`

## `Access-Control-Max-Age`

The `Access-Control-Max-Age` response header indicates how long the results of a preflight request (that is the information contained in the `Access-Control-Allow-Methods` and `Access-Control-Allow-Header`s headers) can be cached.

## `Access-Control-Allow-Credentials`

The `Access-Control-Allow-Credentials` response header tells browsers whether to expose the response to the frontend JavaScript code when the request's credentials mode (`Request.credentials`) is `include`.

## Questions

Q: Which requests required a prefligh request?
A: All non-simple requests. Simple requests are `GET`, `POST`, `HEAD` with safelisted headers. 