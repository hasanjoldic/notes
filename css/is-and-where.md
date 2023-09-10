# `:is()` and `:where()`

```css
h1 > b, h2 > b, h3 > b, h4 > b, h5 > b, h6 > b {
  color: hotpink;
}
```

Instead, you could use `:is()` and improve legibility while avoiding a long selector:

```css
:is(h1,h2,h3,h4,h5,h6) > b {
  color: hotpink;
}
```

```css
article > :is(p,blockquote) {
  color: black;
}

:is(.dark-theme.hero > h1) {
  font-weight: bold;
}

article:is(.dark-theme:not(main .hero)) {
  font-size: 2rem;
}
```

> Normally, when using a , to create a list of selectors, if any of the selectors are invalid, all of the selectors are invalidated and the list will fail to match elements. That is to say they are not forgiving of errors. :is() and :where() though are forgiving, and can get you out of a bind!

## The difference between `:is()` and `:where()` #

- `:where()` has no specificity - `:where()` squashes all the specificity in the selector list passed as functional parameters. This is a first of its kind selector feature.
- `:is()` takes the specificity of its most specific selector - `:is(a,div,#id)` has a specificity score of an ID, 100 points.
