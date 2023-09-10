# `accent-color`

## Supported elements

Currently, only four elements will tint via the `accent-color` property:

- `checkbox`
- `radio`
- `range`
- `progress`

## More tinting

How to tint more than these four form elements?

- the focus ring
- text selection highlights
- list markers
- arrow indicators (Webkit only)
- scrollbar thumb (Firefox only)

```css
html {
  --brand: hotpink;
  scrollbar-color: hotpink Canvas;
}

:root { accent-color: var(--brand); }
:focus-visible { outline-color: var(--brand); }
::selection { background-color: var(--brand); }
::marker { color: var(--brand); }

:is(
  ::-webkit-calendar-picker-indicator,
  ::-webkit-clear-button,
  ::-webkit-inner-spin-button,
  ::-webkit-outer-spin-button
) {
  color: var(--brand);
}
```
