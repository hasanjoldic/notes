# Links and Buttons

## Links

Quick guidelines on when to use each:

- Are you giving a user a way to go to another page or a different part of the same page? Use a link (`<a href="/somewhere">link</a>`)
- Are you making a JavaScript-powered clickable action? Use a button (`<button type="button">button</button>`)
- Are you submitting a form? Use a submit input (`<input type="submit" value="Submit">`)

### Opening links in a new tab

> Do not arbitrarily open links in a new window or tab. If you are required to do so anyway, inform users in visible text.

Don’t use it:

- You or your client prefer it personally.
- You’re trying to beef up your time on site metric.
- You’re distinguishing between internal and external links or content types.
- It’s your way out of dealing with infinite scroll trickiness.

Do use it:

- A user is doing something on the current page, like actively playing media or has unsaved work.
- You have some obscure technical reason where you are forced to (even then you’re still probably the rule, not the exception).

### Jump link

Links can also be "hash links" or "jump links" by starting with a #:

```html
<a href="#section-2">Section Two</a>
<!-- will jump to... -->
<section id="section-2"></section>
```

Clicking that link will "jump" (scroll) to the first element in the DOM with an ID that matches, like the section element above.

### The `rel` attribute

- `rel="alternate"`: Alternate version of the document.
- `rel="author"`: Author of the document.
- `rel="help"`: A resource for help with the document.
- `rel="license"`: License and legal information.
- `rel="manifest"`: Web App Manifest document.
- `rel="next"`: Next document in the series.
- `rel="prev"`: Previous document in the series.
- `rel="search"`: A document meant to perform a search in the current document.

There are also some rel attributes specifically to inform search engines:

- `rel="sponsored"`: Mark links that are advertisements or paid placements (commonly called paid links) as sponsored.
- `rel="ugc"`: For not-particularly-trusted user-generated content, like comments and forum posts.
- `rel="nofollow"`: Tell the search engine to ignore this and not associate this site with where this links to.

And also some rel attributes that are most security-focused:

- `rel="noopener"`: Prevent a new tab from using the JavaScript `window.opener` feature, which could potentially access the page containing the link (your site) to perform malicious things, like stealing information or sharing infected code. Using this with target="_blank" is often a good idea.
- `rel="noreferrer"`: Prevent other sites or tracking services (e.g. Google Analytics) from identifying your page as the source of clicked link.

### ARIA roles

The default role of a link is `link`, so you do not need to do:

```html
<a role="link" href="/">Link</a>
```

A useful ARIA role to indicate the current page, like:

```html
<a href="/" aria-current="page">Home</a>
<a href="/contact">Contact</a>
<a href="/about">About/a></a>
```

### Link states

Links are focusable elements. In other words, they can be selected using the Tab key on a keyboard. Links are perhaps the most common element where you’ll very consciously design the different states, including a `:focus` state.

- `:hover`: For styling when a mouse pointer is over the link.
- `:visited`: For styling when the link has been followed, as best as the browser can remember. It has limited styling ability due to security.
- `:link`: For styling when a link has not been visited.
- `:active`: For styling when the link is pressed (e.g. the mouse button is down or the element is being tapped on a touch screen).
- `:focus`: Very important! Links should always have a focus style. If you choose to remove the default blue outline that most browsers apply, also use this selector to re-apply a visually obvious focus style.

### Styling links for print

CSS has an at-rule for declaring styles that only take effect on printed media:

```css
@media print {
  /* For links in content, visually display the link */ 
  article a::after { 
    content: " (" attr(href) ")";
  }
}
```

### Resetting styles

If you needed to take all the styling off a link (or really any other element for that matter), CSS provides a way to remove all the styles using the all property.

```css
.special-area a {
  all: unset;
  all: revert;
  
  /* Start from scratch */
  color: purple;
}
```

### `preventDefault`

```js
const jumpLinks = document.querySelectorAll("a[href^='#']");

jumpLinks.forEach(link => {
 link.addEventListener('click', event => {
    event.preventDefault();
    // Do something else instead, like handle the navigation behavior yourself
  });
});
```

This kind of thing is at the core of how "Single Page Apps" (SPAs) work. They intercept the clicks so browsers don’t take over and handle the navigation. 

### Accessibility considerations

- You don’t need text like "Link" or "Go to" in the link text itself. Make the text meaningful ("documentation" instead of "click here").
- Links already have an ARIA role by default (role="link") so there’s no need to explicitly set it.
- Try not to use the URL itself as the text (`<a href="google.com">google.com</a>`)
- Links are generally blue and generally underlined and that’s generally good.
- All images in content should have alt text anyway, but doubly so when the image is wrapped in a link with otherwise no text.

## Buttons

Buttons are for triggering actions. When do you use the `<button>` element? A good rule is to use a button when there is “no meaningful href.” Here’s another way to think of that: if clicking it doesn’t do anything without JavaScript, it should be a `<button>`.

### Inside forms

Buttons inside of a `<form>` do something by default: they submit the form! They can also reset it, like their input counterparts. The type attributes matter:

```html
<form action="/" method="POST">
  <input type="text" name="name" id="name">
  <button>Submit</button>

  <!-- If you want to be more explicit... -->
  <button type="submit">Submit</button>

  <!-- ...or clear the form inputs back to their initial values -->
  <button type="reset">Reset</button>

  <!-- This prevents a `submit` action from firing which may be useful sometimes inside a form -->
  <button type="button">Non-submitting button</button>
</form>
```

Speaking of forms, buttons have some neat tricks up their sleeve where they can override attributes of the `<form>` itself.

```html
<form action="/" method="get">

  <!-- override the action -->
  <button formaction="/elsewhere/" type="submit">Submit to elsewhere</button>

  <!-- override encytype -->
  <button formenctype="multipart/form-data" type="submit"></button>

  <!-- override method -->
  <button formmethod="post" type="submit"></button>

  <!-- do not validate fields -->
  <button formnovalidate type="submit"></button>

  <!-- override target e.g. open in new tab -->
  <button formtarget="_blank" type="submit"></button>

</form>
```

### Autofocus

Since buttons are focusable elements, we can automatically focus on them when the page loads using the autofocus attribute:

```html
<div class="modal">

  <h2>Save document?</h2>

  <button>Cancel</button>
  <button autofocus>OK</button>
</div>
```

Perhaps you’d do that inside of a modal dialog where one of the actions is a default action and it helps the UX (e.g. you can press Enter to dismiss the modal).

### ARIA

Buttons already have the role they need (`role="button"`). But there are some other ARIA attributes that are related to buttons:

- `aria-pressed`: Turns a button into a toggle, between `aria-pressed="true"` and `aria-pressed="false"`, which can also be done with `role="switch"` and `aria-checked="true"`.
- `aria-expanded`: If the button controls the open/closed state of another element (like a dropdown menu), you apply this attribute to indicate that like `aria-expanded="true"`.
- `aria-label`: Overrides the text within the button. This is useful for labeling buttons that otherwise don’t have text, but you’re still probably better off using a visually-hidden class so it can be translated.
- `aria-labelledby`: Points to an element that will act as the label for the button.
