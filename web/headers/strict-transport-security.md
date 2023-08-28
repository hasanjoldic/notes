# Strict-Transport-Security

The HTTP `Strict-Transport-Security` response header (often abbreviated as HSTS) informs browsers that the site should only be accessed using HTTPS, and that any future attempts to access it using HTTP should automatically be converted to HTTPS.

> Note: This is more secure than simply configuring a HTTP to HTTPS (301) redirect on your server, where the initial HTTP connection is still vulnerable to a man-in-the-middle attack.

## Example:

```http
Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
```

Google maintains an [HSTS preload service](https://hstspreload.org/). By following the guidelines and successfully submitting your domain, you can ensure that browsers will connect to your domain only via secure connections. While the service is hosted by Google, all browsers are using this preload list. However, it is not part of the HSTS specification and should not be treated as official.
