# Responsive web design basics

Pages optimized for a variety of devices must include a meta viewport tag in the head of the document. **A meta viewport tag gives the browser instructions on how to control the page's dimensions and scaling.**

To attempt to provide the best experience, mobile browsers render the page at a desktop screen width (usually about `980px`, though this varies across devices), and then try to make the content look better by increasing font sizes and scaling the content to fit the screen. This means that font sizes may appear inconsistent to users, who may have to double-tap or pinch-to-zoom in order to see and interact with the content.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    …
    <meta name="viewport" content="width=device-width, initial-scale=1">
    …
  </head>
  …
```
