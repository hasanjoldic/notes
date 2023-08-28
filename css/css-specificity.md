# CSS specificity

Highest to lowest:

0. `!important` suffix - can only be overriden with another `!important` which is defined later in the CSS
1. inline style
2. id
3. class, pseudo-class, attribute
4. element, pseudo-element
5. universtal selector (`*`)
6. `:not()` and `:has()` pseudo-class - adds no specificity by itself, only what's inside adds specificity
