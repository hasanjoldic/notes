# Border animations

[CodePen](https://codepen.io/Chokcoco/pen/YzGdEMZ)
[dev.to](https://dev.to/chokcoco/fantastic-css-border-animation-5166)

```css
*, *::before, *::after {
  box-sizing: border-box;
}

@keyframes rotate {
  100% {
    transform: rotate(1turn);
  }
}

.rainbow {
  position: relative;
  width: 400px;
  height: 400px;
  overflow: hidden;
  padding: 2rem;
  border-radius: 8px;
  
  &::before {
    content: '';
    position: absolute;
    z-index: -2;
    left: -50%;
    top: -50%;
    width: 200%;
    height: 200%;
    background-color: #399953;
    background-repeat: no-repeat;
    background-size: 50% 50%, 50% 50%;
    background-position: 0 0, 100% 0, 100% 100%, 0 100%;
    background-image: linear-gradient(green, green), linear-gradient(yellow, yellow), linear-gradient(red, red), linear-gradient(blue, blue);
    animation: rotate 5s linear infinite;
    border-radius: 10px;
  }
  
  &::after {
    content: '';
    position: absolute;
    z-index: -1;
    left: 6px;
    top: 6px;
    width: calc(100% - 12px);
    height: calc(100% - 12px);
    background: white;
    border-radius: 4px;
  }
}
```
