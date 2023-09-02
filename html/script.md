# `<script>` HTML element

Attributes:

- `src`

  This attribute specifies the URI of an external script.

- `async`

  Prevents blocking of the main thread. The code will be evaluated as soonas it's available - it won't wait until the browser has finished parsing all of HTML.

- `defer`

  - The script will be executed after the document has been parsed, but before firing `DOMContentLoaded``.

  - Scripts with the `defer` attribute will prevent the `DOMContentLoaded` event from firing until the script has loaded and finished evaluating.

  - Scripts with the `defer` attribute will execute in the order in which they appear in the document.
  
  > Warning: This attribute must not be used if the src attribute is absent (i.e. for inline scripts), in this case it would have no effect.
  > The defer attribute has no effect on module scripts — they defer by default.

## Notes

- Scripts without the `async` or `defer` attribute are fetched and executed immediately before the browser continues to parse the page.

- The script should be served with the `text/javascript` MIME type, but browsers are lenient and only block them if the script is served with an image type (`image/*`), a video type (`video/*`), an audio type (`audio/*`), or `text/csv`. If the script is blocked, an error event is sent to the element; otherwise, a load event is sent.

## Best practices

- Always set `async` and `defer` attributes in `script` elements, unless:

  - Script is an inline script. In that case `defer` will have no effect.
  - Script needs to be evaluated before HTML can be parsed.
  - Scripts need to be evaluated in a specific order.
