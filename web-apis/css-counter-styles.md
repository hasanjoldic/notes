# CSS counter styles

[MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/@counter-style)
[CSS-Tricks](https://css-tricks.com/css-counters-custom-list-number-styling/)

The `@counter-style` CSS at-rule lets you define counter styles that are not part of the predefined set of styles.

```css
@counter-style thumbs {
  system: cyclic;
  symbols: "\1F44D";
  suffix: " ";
}

ul {
  list-style: thumbs;
}
```
