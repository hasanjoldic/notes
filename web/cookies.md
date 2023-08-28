# Cookies

We have `Set-Cookie` header from the server, and `Cookie` header from the client.

The `Set-Cookie` HTTP response header sends cookies from the server to the user agent.

This instructs the server sending headers to tell the client to store a pair of cookies:

```http
HTTP/2.0 200 OK
Content-Type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry

[page content]
```

Then, with every subsequent request to the server, the browser sends all previously stored cookies back to the server using the `Cookie` header.

```http
GET /sample_page.html HTTP/2.0
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```

## Best practices

- Use the `HttpOnly` attribute to prevent access to cookie values via JavaScript.
- Use `Secure` to prevent the cookie being sent on non HTTPS connections.
- Cookies that are used for sensitive information (such as indicating authentication) should have the `SameSite` attribute set to `Strict` or `Lax`. This ensures that the authentication cookie isn't sent with cross-site requests. __Setting it to `Lax` is recommended.__ `Lax` will allow your site to be reachable by users clicking links in other sites, but it will prevent POST requests from forms.
- `Domain` should always be set to indicate whether it should be valid only for a specific domain or for all of its subdomains as well.

Example:

```http
Set-Cookie: id=a3fWa; Domain="www.website.dom"; Secure; HttpOnly
```
