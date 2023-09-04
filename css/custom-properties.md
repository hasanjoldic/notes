# Custom Properties

A custom property is most commonly thought of as a variable in CSS.

```css
.card {
  --spacing: 1.2rem;
  padding: var(--spacing);
  margin-bottom: var(--spacing);
}
```

> **Custom properties must be within a selector and start with two dashes (--)**

**You can set the value of a custom property with another custom property:**

```css
html {
  --red: #a24e34;
  --green: #01f3e6;
  --yellow: #f0e765;

  --error: var(--red);
  --errorBorder: 1px dashed var(--red);
  --ok: var(--green);
  --warning: var(--yellow);
}
```

```css
button {
  --spread: 5px;
  box-shadow: 0 0 20px var(--spread) black;
}
button:hover {
  --spread: 10px;
}
```

```css
.grid {
  display: grid;
  --edge: 10px;
  grid-template-columns: var(--edge) 1fr var(--edge);
}
@media (min-width: 1000px) {
  .grid {
     --edge: 15%;
   }
}
```

## Concatenation of unit types

There are times when combining parts of values doesn’t work quite how you might hope. For example, you can’t make 24px by smashing 24 and px together. It can be done though, by multiplying the raw number by a number value with a unit.

```css
body {
  --value: 24;
  --unit: px;
  
  /* Nope */
  font-size: var(--value) + var(--unit);
  
  /* Yep */
  font-size: calc(var(--value) * 1px);

  /* Yep */
  --pixel_converter: 1px;
  font-size: calc(var(--value) * var(--pixel_converter));
}
```

## Using the cascade

```css
body {
  --background: white;
}
.sidebar {
  --background: gray;
}
.module {
  background: var(--background);
}
```

```html
<body> <!-- --background: white -->

  <main>
    <div class="module">
      I will have a white background.
    </div>
  <main>

  <aside class="sidebar"> <!-- --background: gray -->
    <div class="module">
      I will have a gray background.
    </div>
  </aside>

</body>
```

The "module" in the sidebar has a gray background because custom properties (like many other CSS properties) inherit through the HTML structure. Each module takes the `--background` value from the nearest "ancestor" where it’s been defined in CSS.

Media queries don’t change specificity, but they often come later (or lower) in the CSS file than where the original selector sets a value, which also means a custom property will be overridden inside the media query:

```css
body {
  --size: 16px;
  font-size: var(--size);
}
@media (max-width: 600px) {
  body {
    --size: 14px;
  } 
}
```

Media queries aren’t only for screen sizes. They can be used for things like accessibility preferences. For example, dark mode:

```css
body {
  --bg-color: white; 
  --text-color: black;

  background-color: var(--bg-color);
  color: var(--text-color);
}

/* If the user's preferred color scheme is dark */
@media screen and (prefers-color-scheme: dark) {
  body {
    --bg-color: black;
    --text-color: white;
  }
}
```

## The `:root` thing

```css
:root {
  --color: red;
}

/* ...is largely the same as writing: */
html {
  --color: red;
}

/* ...except :root has higher specificity, so remember that! */
```

## Custom property fallbacks

Here we’re setting a `scale()` transform function to a custom property, but there is a comma-separated second value of `1.2`. That `1.2` value will be used if `--scale` is not set.

```css
.bigger {
  transform: scale(var(--scale, 1.2));
}

html {
  font-family: var(--fonts, Helvetica, Arial, sans-serif);
}
```

## `@property`

> The `@property` “at-rule” in CSS allows you to declare the type of a custom property, as well its as initial value and whether it inherits or not.

```css
@property --x {
  syntax: '<number>';
  inherits: false;
  initial-value: 42;
}
```

It’s sort of like you’re creating an actual CSS property and have the ability to define what it’s called, it’s syntax, how it interacts with the cascade, and its initial value.

Valid types:

- length
- number
- percentage
- length-percentage
- color
- image
- url
- integer
- angle
- time
- resolution
- transform-list
- transform-function
- custom-ident (a custom identifier string)
