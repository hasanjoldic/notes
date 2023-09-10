# Data Attributes

It is discouraged making up your own attributes, or repurposing existing attributes for unrelated functionality.

```html
<!-- `highlight` is not an HTML attribute -->
<div highlight="true"></div>

<!-- `large` is not a valid value of `width` -->
<div width="large">
```

But!, we can make up your own attributes. We just need to prefix them with `data-`.

Examples

```html
<button data-id="435432343">♡</button>
```

That button could have a click handler on it which performs an Ajax request to the server to increment the number of likes in a database on click. It knows which record to update because it gets it from the data attribute.
