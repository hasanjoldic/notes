# Web development checklist

- console.log errors

Planning:

- Define users to be able to define the target environments
  - mobile, desktop?
  - chromium, firefox, safari?
  - internal, B2B, B2C?

Meta:

- (HIGH) [Viewport meta tag](https://webdesign.tutsplus.com/quick-tip-dont-forget-the-viewport-meta-tag--webdesign-5972a)
- (MED) Custom 404 page
- (MED) Custom 500 page
- (MED) robots.txt
- (MED) sitemap.xml
- (MED) Favicon
- (MED) [Safari](https://developer.apple.com/library/archive/documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html)
- (LOW) humans.txt

Design and styles

- (LOW) [Print styles](https://developer.mozilla.org/en-US/docs/Web/Guide/Printing)

- [Use correct input types](https://www.htmlgoodies.com/guides/html5-forms-how-to-use-the-new-email-url-and-telephone-input-types/)

- Test on real devices

Security:

- (HIGH) XSS
- (HIGH) CSRF
- (HIGH) [Redirect all HTTP traffic to HTTPS](https://infosec.mozilla.org/guidelines/web_security#http-redirections)
- (HIGH) [CORS](https://infosec.mozilla.org/guidelines/web_security#cross-origin-resource-sharing)
- (HIGH) [CSP](https://infosec.mozilla.org/guidelines/web_security#content-security-policy)
- (HIGH) Set `SameSite=lax; HttpOnly; Secure;` in all [Cookies](https://infosec.mozilla.org/guidelines/web_security#cookies)
- (HIGH) Set [Referrer-Policy](https://infosec.mozilla.org/guidelines/web_security#referrer-policy) to `same-origin`
- (HIGH) [Subresource Integrity](https://infosec.mozilla.org/guidelines/web_security#subresource-integrity)
- (HIGH) [X-Content-Type-Options](https://infosec.mozilla.org/guidelines/web_security#x-content-type-options)
- (HIGH) [X-Frame-Options: DENY](https://infosec.mozilla.org/guidelines/web_security#x-frame-options)
- (HIGH) Returning sensitive data to unauthorized clients
- (LOW) [HSTS](https://infosec.mozilla.org/guidelines/web_security#http-strict-transport-security)
- (LOW) [X-XSS-Protection: 0](https://infosec.mozilla.org/guidelines/web_security#x-xss-protection)

SEO:

- (LOW) [Structured data for Google search](https://developers.google.com/search/docs/appearance/structured-data/intro-structured-data)
- (LOW) [Twitter cards](https://developer.twitter.com/en/docs/twitter-for-websites/cards/overview/abouts-cards)
- (LOW) [Sharing on Facebook](https://developers.facebook.com/docs/sharing/webmasters#markup)

Legal:

- GDPR

Deployment and reliability:

- (HIGH) Uptime monitoring
- (HIGH) Error monitoring
- (HIGH) Restart app, db, workers on crash and reboot
- (HIGH) Database backups and restore
- (HIGH) DB transactions

<https://frontendchecklist.io>

Performance:

- (HIGH) Bundle size
- (HIGH) Data usage
- (HIGH) FPS

<!DOCTYPE html>
<html lang="en">
<meta charset="UTF-8">
<meta name="description" content="Description">
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Title</title> - on all pages

<meta name="apple-touch...
<meta name="apple-mobile...
<meta name="msapplication...

rss feed

Inline critical CSS: The inline critical CSS is correctly injected in the HEAD.
CSS order: All CSS files are loaded before any JavaScript files in the HEAD

[Facebook](https://developers.facebook.com/docs/sharing/webmasters/)
[Apple](https://developer.apple.com/library/archive/documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html)
[Twitter](https://developer.twitter.com/en/docs/twitter-for-websites/cards/guides/getting-started)

Noopener: In case you are using external links with target="_blank", your link should have a rel="noopener" attribute to prevent tab nabbing. If you need to support older versions of Firefox, use rel="noopener noreferrer"

W3C compliant: All pages need to be tested with the W3C validator to identify possible issues in the HTML code. [link](https://validator.w3.org/)

HTML Lint: Use tools to analyze any issueswith HTML code.

Adblockers test: Your website shows your content correctly with adblockers enabled

Fonts:

Webfont format: WOFF, WOFF2 and TTF are supported by all modern browsers.

Webfont size: Webfont sizes don't exceed 100 KB (all variants included).

Webfont loader: Control loading behavior with a webfont loader.
https://css-tricks.com/loading-web-fonts-with-the-web-font-loader/

CSS:

Reset CSS: A CSS reset (reset, normalize or reboot) is used and up to date.

Embedded or inline CSS: Avoid at all cost embeding CSS in <style> tags or using inline CSS

Non-blocking: CSS files need to be non-blocking to prevent the DOM from taking time to load.
https://developer.mozilla.org/en-US/docs/Learn/Performance/CSS

Stylelint: All CSS or SCSS files are without any errors.

Javascript:

noscript tag: Use `<noscript>` tag in the HTML body if a script type on the page is unsupported or if scripting is currently turned off in the browser. This will be helpful in client-side rendering heavy apps such as React.js.

Non-blocking: JavaScript files are loaded asynchronously using async or deferred using defer attribute.

Modernizr: If you need to target some specific features you can use a custom Modernizr to add classes in your <html> tag.

IMAGES:

Optimization: All images are optimized to be rendered in the browser. WebP format could be used for critical pages (like Homepage)

Picture/Srcset: You use picture/srcset to provide the most appropriate image for the current viewport of the user.

Retina: You provide layout images 2x or 3x, support retina display.

Width and Height: Set width and height attributes on <img> if the final rendered image size is known (can be omitted for CSS sizing).

Alternative text: All <img> have an alternative text which describe the image visually.

Lazy loading: Images are lazyloaded (A noscript fallback is always provided).

PERFORMANCE:

Page weight: The weight of each page is between 0 and 500 KB.

DNS resolution: DNS of third-party services that may be needed are resolved in advance during idle time using dns-prefetch.

Preconnection: DNS lookup, TCP handshake and TLS negotiation with services that will be needed soon is done in advance during idle time using preconnect.

Prefetching: Resources that will be needed soon (e.g. lazy loaded images) are requested in advance during idle time using prefetch.

Preloading: Resources needed in the current page (e.g. scripts placed at the end of <body>) in advance using preload.

