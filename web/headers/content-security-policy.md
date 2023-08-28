# Content security policy (CSP)

CSP allows us to define all allowed resources.

To enable CSP, you need to configure your web server to return the `Content-Security-Policy` HTTP header. 

A primary goal of CSP is to mitigate and report XSS attacks. XSS attacks exploit the browser's trust in the content received from the server.

## Mitigating packet sniffing attacks

In addition to restricting the domains from which content can be loaded, the server can specify which protocols are allowed to be used; for example (and ideally, from a security standpoint), a server can specify that all content must be loaded using HTTPS. A complete data transmission security strategy includes not only enforcing HTTPS for data transfer, but also marking all cookies with the `secure` attribute and providing automatic redirects from HTTP pages to their HTTPS counterparts. Sites may also use the `Strict-Transport-Security` HTTP header to ensure that browsers connect to them only over an encrypted channel.

## Best practices

1. Set `default-src "none"` to disallow all resources by default.
2. Set other resources one by one as needed. Such as: `connect-src`, `script-src`, `img-src`, `style-src`, `font-src`, etc.
