# CSS performance optimization

## [Remove unnecessary styles](https://css-tricks.com/how-do-you-remove-unused-css-from-a-site/)

## Split CSS into separate modules

The simplest way to do this is by splitting up your CSS into separate files and loading only what is needed:

```html
<!-- Loading and parsing styles.css is render-blocking -->
<link rel="stylesheet" href="styles.css" />

<!-- Loading and parsing print.css is not render-blocking -->
<link rel="stylesheet" href="print.css" media="print" />

<!-- Loading and parsing mobile.css is not render-blocking on large screens -->
<link
  rel="stylesheet"
  href="mobile.css"
  media="screen and (max-width: 480px)" />
```

By default, the browser assumes that each specified style sheet is render-blocking. You can tell the browser when a style sheet should be applied by adding a media attribute containing a media query. When the browser sees a style sheet that it only needs to apply in a specific scenario, it still downloads the stylesheet, but doesn't render-block.

## Minify and compress your CSS

## Simplify selectors

People often write selectors that are more complex than needed for applying the required styles. This not only increases file sizes, but also the parsing time for those selectors. For example:

```html
/* Very specific selector */
body div#main-content article.post h2.headline {
  font-size: 24px;
}

/* You probably only need this */
.headline {
  font-size: 24px;
}
```

## Don't apply styles to more elements than needed

A common mistake is to apply styles to all elements using the universal selector, or at least, to more elements than needed. This kind of styling can impact performance negtively, especially on larger sites.

## Preload important assets

You can use `rel="preload"` to turn `<link>` elements into preloaders for critical assets. This includes CSS files, fonts, and images: 

```html
<link rel="preload" href="style.css" as="style" />

<link
  rel="preload"
  href="ComicSans.woff2"
  as="font"
  type="font/woff2"
  crossorigin />

<link
  rel="preload"
  href="bg-image-wide.png"
  as="image"
  media="(min-width: 601px)" />
```

With `preload`, the browser will fetch the referenced resources as soon as possible and make them available in the browser cache so that they will be ready for use sooner when they are referenced in subsequent code.

https://web.dev/preload-critical-assets/

## Font loading

Bear in mind that a font is only loaded when it is actually applied to an element using the `font-family` property, not when it is first referenced using the `@font-face` at-rule:

```html
/* Font not loaded here */
@font-face {
  font-family: "Open Sans";
  src: url("OpenSans-Regular-webfont.woff2") format("woff2");
}

h1,
h2,
h3 {
  /* It is actually loaded here */
  font-family: "Open Sans";
}
```

It can therefore be beneficial to use `rel="preload"` to load important fonts early, so they will be available more quickly when they are actually needed:

```html
<link
  rel="preload"
  href="OpenSans-Regular-webfont.woff2"
  as="font"
  type="font/woff2"
  crossorigin />
```