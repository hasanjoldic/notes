# Back/forward cache (bfcache)

**bfcache is an in-memory cache that stores a complete snapshot of a page (including the JavaScript heap) as the user is navigating away. With the entire page in memory, the browser can quickly and easily restore it if the user decides to return.**

**bfcache is an optimization that browsers do automatically.**

Creating a snapshot of a page in memory, however, involves some complexity in terms of how best to preserve in-progress code. For example, how do you handle setTimeout() calls where the timeout is reached while the page is in the bfcache?  
The answer is that browsers pause running any pending timers or unresolved promises—essentially all pending tasks in the JavaScript task queues—and resume processing tasks when (or if) the page is restored from the bfcache.

In some cases this is fairly low-risk (for example, timeouts or promises), but in other cases it might lead to very confusing or unexpected behavior. For example, if the browser pauses a task that's required as part of an IndexedDB transaction, it can affect other open tabs in the same origin (since the same IndexedDB databases can be accessed by multiple tabs simultaneously). As a result, browsers will generally not attempt to cache pages in the middle of an IndexedDB transaction or using APIs that might affect other pages.

## Optimize your pages for bfcache

### Never use the `unload` event

### Minimize use of `Cache-Control: no-store`

### Update stale or sensitive data after bfcache restore

### Always close open connections before the user navigates away

Browsers will not attempt to put a page in bfcache in the following scenarios:

- open IndexedDB connection
- in-progress fetch() or XMLHttpRequest
- open WebSocket or WebRTC connection
