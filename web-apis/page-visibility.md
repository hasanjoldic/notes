# Page visibility API

[MDN](https://developer.mozilla.org/en-US/docs/Web/API/Page_Visibility_API)

The Page Visibility API provides events you can watch for to know when a document becomes visible or hidden, as well as features to look at the current visibility state of the page.
This is especially useful for saving resources and improving performance by letting a page avoid performing unnecessary tasks when the document isn't visible.

For example, if your web app is playing a video, it can pause the video when the user puts the tab into the background, and resume playback when the user returns to the tab.

## Use cases

- A site has an image carousel that shouldn't advance to the next slide unless the user is viewing the page
- An application showing a dashboard of information doesn't want to poll the server for updates when the page isn't visible
- A page wants to detect when it is being prerendered so it can keep accurate count of page views
- A site wants to switch off sounds when a device is in standby mode (user pushes power button to turn screen off)

## Policies in place to aid background page performance

- Most browsers stop sending requestAnimationFrame() callbacks to background tabs.

- Timers such as setTimeout() are throttled in background/inactive tabs.

- Browsers implement budget-based background timeout throttling:

  - In Firefox, windows in background tabs each have their own time budget in milliseconds — a max and a min value of +50 ms and -150 ms, respectively. Chrome is very similar except that the budget is specified in seconds.

  - Windows are subjected to throttling after 30 seconds, with the same throttling delay rules as specified for window timers. In Chrome, this value is 10 seconds.

  - Timer tasks are only permitted when the budget is non-negative.

  - Once a timer's code has finished running, the duration of time it took to execute is subtracted from its window's timeout budget.

  - The budget regenerates at a rate of 10 ms per second, in both Firefox and Chrome.

Some processes are exempt from this throttling behavior:

- Tabs which are playing audio
- Tabs running code that's using real-time network connections (__WebSockets__ and __WebRTC__)
- IndexedDB processes

## Extensions to other interfaces

### Instance properties

~~Document.hidden Read only Deprecated~~

`Document.visibilityState` Read only

A string indicating the document's current visibility state:

- `visible`

    The page content may be at least partially visible. In practice this means that the page is the foreground tab of a non-minimized window.

- `hidden`

    The page's content is not visible to the user, either due to the document's tab being in the background or part of a window that is minimized, or because the device's screen is off.

### Events

`visibilitychange`

Fired when the content of a tab has become visible or has been hidden.

## Example

### Pausing audio on page hide

```html
<audio
  controls
  src="https://mdn.github.io/webaudio-examples/audio-basics/outfoxing.mp3"></audio>
```

```js
const audio = document.querySelector("audio");

// Handle page visibility change:
// - If the page is hidden, pause the video
// - If the page is shown, play the video
document.addEventListener("visibilitychange", () => {
  if (document.hidden) {
    audio.pause();
  } else {
    audio.play();
  }
});
```
