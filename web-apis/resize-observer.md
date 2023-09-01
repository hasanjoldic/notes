# Resize observer API

[MDN](https://developer.mozilla.org/en-US/docs/Web/API/Resize_Observer_API)

The Resize Observer API provides a __performant__ mechanism by which code can monitor an element for changes to its size, with notifications being delivered to the observer each time the size changes.

Usage is simple, and pretty much the same as other observers, such as __Performance Observer__ or __Intersection Observer__ — you create a new `ResizeObserver` object using the `ResizeObserver()` constructor, then use `ResizeObserver.observe()` to make it look for changes to a specific element's size. A callback function set up inside the constructor then runs every time the size changes, providing access to the new dimensions and allowing you to do anything you like in response to those changes.

## Example

```js
const resizeObserver = new ResizeObserver((entries) => {
  const calcBorderRadius = (size1, size2) =>
    `${Math.min(100, size1 / 10 + size2 / 10)}px`;

  for (const entry of entries) {
    if (entry.borderBoxSize) {
      entry.target.style.borderRadius = calcBorderRadius(
        entry.borderBoxSize[0].inlineSize,
        entry.borderBoxSize[0].blockSize,
      );
    } else {
      entry.target.style.borderRadius = calcBorderRadius(
        entry.contentRect.width,
        entry.contentRect.height,
      );
    }
  }
});

resizeObserver.observe(document.querySelector("div"));
```