# Fullscreen API

[MDN](https://developer.mozilla.org/en-US/docs/Web/API/Fullscreen_API)

The Fullscreen API has no interfaces of its own. Instead, it augments several other interfaces, such as `Document` and `Element`;

```js
function toggleFullScreen() {
  if (!document.fullscreenElement) {
    document.documentElement.requestFullscreen();
  } else if (document.exitFullscreen) {
    document.exitFullscreen();
  }
}

document.addEventListener(
  "keydown",
  (e) => {
    if (e.key === "Enter") {
      toggleFullScreen();
    }
  },
  false,
);
```
