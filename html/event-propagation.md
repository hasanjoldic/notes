# Event propagation

There are 3 phases in event propagation:

    1. Capturing phase – the event goes down to the element.
    2. Target phase – the event reached the target element.
    3. Bubbling phase – the event bubbles up from the element.

Unless the element is the target element, the listener will be called in the bubbling phase.
Most of the time we don't need to change in which phase the listener should be called.
The default way to set a listener to an event:

```javascript
el.addEventListener(type, listener);
```

But, if we want to fire the listener in the capture phase:

```javascript
el.addEventListener(type, listener, true);
```
