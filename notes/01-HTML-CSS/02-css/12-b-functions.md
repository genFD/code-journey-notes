# VARIABLES

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)
  - [Setting a Background Image](#setting-a-background-image)
  - [Calculating Values](#calculating-values)
  - [Min and Max](#min-and-max)
  - [Clamp](#clamp)
  - [Color Functions](#color-functions)
  - [Filter Functions](#filter-functions)
  - [Transform Functions](#transform-functions)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

- Learn how to use CSS variables and functions to create more organized, efficient, and dynamic websites.

## Notes

_tldr_:

### Intro

- In a broad sense, functions are blocks of organized and reusable code that help to perform a single, related action.

- Functions have the benefit of keeping code modular and allowing for a high degree of reusability.

- Functions in CSS are a little more limiting than most programming languages. We cannot create our own functions in CSS but instead have access to a wide variety of predefined functions

To use a function in CSS, follow the standard functional notation syntax:

```css
h1 {
  property: function-name(argument);
}
```

[The full list of functions](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Functions)

### Setting a Background Image

**[⬆ back to top](#table-of-contents)**

- `The url() function` is used to link to external resources and load them into the stylesheet.

- These resources can be images, fonts, other stylesheets, and more.

- The url() function takes a single argument: the location of the resource in `string format`.

- The location of the linked resource can be provided as absolute or relative paths.

<details>
<summary>
Absolute vs Relative
</summary>
- if the path starts with slash "/", the first slash denotes root. The rest of the slashes in the path are just separators. The Absolute path always starts from the root directory (/).
`/home/abhishek/scripts/my_scripts.sh.`

![Absolute path](/Notes/notes/01-HTML-CSS/assets/path-linux.png)

- A relative path starts from the current directory.

For example, if you are in the `/home` directory and you want to access the `my_scripts.sh` file, you can use `abhishek/scripts/my_scripts.sh.`

</details>

```css
.main-banner {
  background: url('images/bg-image.jpg');
  background-repeat: no-repeat;
  background-position: right;
}
```

### Calculating Values

**[⬆ back to top](#table-of-contents)**

- We use the `calc()` function to perform dynamic mathematical calculations to set property values .

- The `calc()` function takes a mathematical expression as its argument and returns the calculated value

- We can use the `calc()` function as a value of CSS properties that expect numeric values.

- The `calc()` function can be used to perform mathematical operations on a mix of units (rem, vw, px, etc).

- When performing addition `(+)` or subtraction`(-)`, both numbers being operated on must have specified units.

- Division `(/)` requires the divisor (second operand) to be a unit-less number, and multiplication `(*)` requires one of the operands to be unit-less.

- Keep in mind that the `calc()` function `requires whitespace between the operator and the numbers in the expression`

- The function can also be used as one of the multiple values for a property or argument in another function.

```css
.item {
  margin: 15px calc(1.5vw + 5px);
}
```

- we set the top and bottom margins of the .item selector to be 15px

- use the `calc()` function to set the left and right margin values by adding 1.5vw and 5px

### min() and max()

**[⬆ back to top](#table-of-contents)**

- If we want to create responsive elements, the min() and max() functions are great solutions for setting case-specific design constraints.

- The min() function will select the smallest value from a range of values and set that as the associated property’s value

- The max() function will select the largest value from a range of values, which will be used as the associated property’s value.

```css
.content {
  width: 50vw;
  max-width: 500px;
}
```

- the width of the `.content` class is set to be 50vw.
- If the viewport width is greater than 1000 pixels (meaning that 50vw will be greater than 500px)
- the width of the content class will be set to a maximum width of 500px.

- We can simplify the above code using `the min()` function and passing both values for the width and `max-width` property as the function’s arguments.

```css
.content {
  width: min(500px, 50vw);
}
```

- Now the min() function chooses the smaller value of two values given as arguments—in other words, the min() function defines the maximum width the content class can have.

- When the viewport width is smaller than 1000 pixels, the maximum width of the content class is 50vw

- When the viewport width is greater than 1000 pixels, the maximum width is 500px.

```css
.content {
  width: max(500px, 50vw);
}
```

- If we were to replace the `min()` function in the above example with the max() function, then the width of the content class would be 500px if the viewport with is smaller than 1000 pixels.

- If the viewport width is greater than 1000 pixels, the width of the .content class will take up a minimum of 50vw.

### Clamp()

**[⬆ back to top](#table-of-contents)**

- Sometimes we will want to design elements to dynamically scale but also stay confined between an upper and lower bound. The clamp() function is ideal for achieving this!

- the clamp() function enables a specified value to be kept within an upper and lower bound.

```css
.main-text {
  font-size: clamp(12px, 1.5vw, 48px);
}
```

The clamp() function takes three parameters in a specific order:

1. The first argument specifies `the minimum value`. If the preferred value, given as the second argument, is less than this value, then the minimum value will be used. In the code snippet above, 12px is given as the minimum value of the font-size property.

2. The second argument specifies the preferred value. This value is used as long as it is greater than the value of the first argument (lower bound) and less than the value of the third argument (upper bound). In the code snippet above, 1.5vw is given as the preferred value of the font-size property.

3. The third argument specifies the maximum value. This value is the largest value that the property will be set to. In the code snippet above, 48px is given as the maximum value of the font-size property.

### Color Functions

**[⬆ back to top](#table-of-contents)**

These functions can only be used as values for color properties.

The main color functions are:

- The rgb() function which defines color using the standard RGB (red, green, and blue) model.

- The rgba() function, which is similar to the rgb() function except it also defines an alpha channel to specify the color’s opacity level.

- The hsl() function. This function defines color using hue, saturation and lightness.

- The hsla() function. Again, this function is similar to the hsl() function except it also defines an alpha channel that specifies the color’s opacity level.

### Filter Functions

**[⬆ back to top](#table-of-contents)**

- Like color functions, there are CSS functions specifically for the `filter` and `backdrop-filter` properties. These functions create a variety of visual effects for desired elements

- We can use the `brightness()` function for the filter `backdrop-filter` property to affect an element’s overall brightness by applying a linear multiplier to it.

- The `brightness()` function takes a single argument for the amount, which can be either a number or percentage

- Any value under 100% or 1.0 darkens the element, and any value over 100% or 1.0 will brighten the element. The default value of brightness is 100% or 1.0.

```css
.background-img {
  filter: brightness(0.6);
}
```

- The blur() function applies a [Gaussian blur](https://en.wikipedia.org/wiki/Gaussian_blur) to a specified element.

- The blur() function takes a single argument for the radius of the blur specified as a length. The argument of this function cannot be unitless unless a blur amount of 0 is being set.

- `drop-shadow()`. This function applies a drop shadow effect to the desired element. Take a look at the syntax below:

```css
drop-shadow(offset-x offset-y blur-radius color)
```

1. Both offset-x and offset-y are required arguments that determine the horizontal and vertical offset respectively.
2. While blur-radius is an optional argument that determines the shadow’s blur radius—the larger the value, the more blurred the shadow.
3. Finally, the color argument is also optional and determines the shadow’s color. Notice that it is not necessary to separate each of the function’s arguments with commas.

```css
button {
  filter: drop-shadow(-10px 5px 0.2rem rgba(50, 200, 210, 0.6));
}
```

- the horizontal offset is set to -10px
- the vertical offset to 5px
- It has a blur radius of 0.2rem
- has a color of rgba(50, 200, 210, 0.6)

The drop shadow created on the button is offset vertically to the bottom and to the left. It also has a light blue color with 60% opacity.

There are more CSS functions that can be used with the filter and backdrop-filter properties—take a look at the [full list](https://developer.mozilla.org/en-US/docs/Web/CSS/filter) to learn more.

### Transform Functions

**[⬆ back to top](#table-of-contents)**

- We can transform any HTML element using the `transform property` combined with CSS functions.

- The `scale()` function resizes an element, both horizontally and vertically, on a 2D plane.It can take either one or two parameters. If only one argument is given, scale(2) for instance, then the element will grow proportionally on both the x and y-axis. If you only want to scale an element on one of the two axes, either the scaleX() or the scaleY() function can be used.

- The `rotate()` function can be used for the transform property to rotate an element around a fixed point on a given 2D plane. The function accepts a single argument for the angle, which must be in degrees specified with the `deg`unit. Any positive angle means clockwise rotation, and a negative angle means counter-clockwise rotation. It is important to note that the origin of rotation defaults to the center of the element being rotated. For example, using rotate`(180deg)` as the value of transform property would rotate an element at its center, 180 degrees clockwise.

- the `translate()` function moves an element from its initial position to another position on the page specified as the function’s arguments.The function can accept either one or two arguments—if one argument is provided, then the function will translate the element along the only x-axis by the specified amount. If two arguments are given, the element translates along the x-axis by the amount specified by the first argument and along the y-axis by the amount specified as the second argument.

```css
shifted {
  transform: translate(0px, 100px);
}
```

- we wanted to shift the shifted element down the screen by 100px,The function’s x-axis argument was then set to be 0px and the y-axis to be 100px

### Lessons code for postcard

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  font-size: 10px;
}

a {
  text-decoration: none;
  color: inherit;
}

body {
  font-family: 'Helvetica', 'sans-serif';
  font-size: 1.6rem;
  color: #333;
  width: 100%;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0 1.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0 1.5rem;
  margin-top: 20px;
  margin-bottom: 10px;
  background: white;
}

.main-bg-image {
  width: 100%;
  height: 110%;
  position: absolute;
  top: 0;
  left: 0;
  background: url(assets/main-bg-image.jpg) no-repeat;
  background-size: cover;
  filter: brightness(0.6);
}

.card {
  padding-top: 5%;
  display: flex;
  flex-direction: column;
  background: #fff;
  box-shadow: 2px 0.8rem 1rem rgba(0, 0, 0, 0.09);
  border-radius: 8px 8px 0px 0px;
  width: clamp(100px, 90%, 600px);
}

.under {
  letter-spacing: 0px;
}

.image-info {
  height: 25rem;
  position: relative;
  z-index: 1;
  overflow: hidden;
}

.bg-image {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  background-color: gray;
  z-index: -1;
  transition: all 2s ease-in-out;
  border-radius: 8px 8px 0px 0px;
  background-image: url(images/argentina.jpg);
  background-position: center;
  background-size: cover;
}

.card:hover .bg-image {
  filter: saturate(0.2);
  transform: scale(1.4);
}

.location-details {
  width: 100%;
  height: 100%;
  font-size: 1.6rem;
  font-weight: bold;
  color: #fff;
  position: relative;
  top: 0;
  left: 0;
  letter-spacing: 2px;
  border-radius: 8px 8px 0px 0px;
  transform: translate(60%, 50%);
}

.location-details i {
  margin-right: 2rem;
}

.send-to {
  display: inline-block;
  margin-top: 2rem;
}
.from {
  display: block;
  margin-bottom: 1rem;
  margin-top: 1rem;
}

.date {
  display: block;
}

.card-body {
  background: rgb(255, 247, 255);
  padding: 2rem 3rem;
  position: relative;
}

.card-body::before,
.card-body::after {
  content: '';
  position: absolute;
  border: 2px solid hsl(25, 47%, 86%);
}

.card-body::before {
  top: 1.2rem;
  bottom: 1.2rem;
  right: 1.2rem;
}

.card-body::before {
  bottom: 1.2rem;
  left: 1.2rem;
  top: 1.2rem;
}

.card-body::after {
  top: 1.2rem;
  left: 1.2rem;
  right: 1.2rem;
}

.card-body::after {
  top: 1.2rem;
  left: 1.2rem;
  bottom: 1.2rem;
}

.card-title {
  padding: 5px 0;
  font-size: calc(2vw + 1.5rem);
  line-height: 1;
  text-align: center;
  margin-bottom: 0.5em;
  position: relative;
  z-index: 1;
}

.header-modifier {
  display: inline-block;
  color: #6cace4;
  transition: transform 0.8s ease-in-out;
  filter: drop-shadow(10px 5px 0.2rem rgba(236, 217, 203, 0.7));
}

.header-modifier:hover {
  transform: rotate(5deg);
}

.card-text {
  display: inline-block;
  position: relative;
  text-align: center;
  width: clamp(50px, 80%, 600px);
  border-radius: 8px;
  padding: 5px 0;
  overflow-y: hidden;
  left: calc(50% - 40%);
  font-size: clamp(1.5rem, 1.8vw, 1.8rem);
}

ul.activities {
  align-items: center;
  list-style: circle;
  position: relative;
  font-weight: bold;
  color: hsla(208, 69%, 66%, 0.6);
  margin: calc(1vw + 0.5rem) 2rem;
}

li {
  padding: 5px 0;
}

.activities a:hover {
  cursor: pointer;
}

.photo-link {
  text-align: right;
  font-weight: bold;
  margin-top: 1rem;
}

.photo-link a {
  color: rgba(236, 217, 203, 0.9);
  position: relative;
}
```

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="style.css" />
    <title>CSS Functions</title>
  </head>

  <body>
    <div class="main-bg-image"></div>
    <div class="card">
      <div class="image-info">
        <div class="bg-image"></div>
        <div class="location-details">
          <div class="send-to">
            <i></i>To:<span class="under"> ________</span>
          </div>
          <div class="from">
            <i></i>From:<span class="under"> ________</span>
          </div>
          <div class="date">
            <i></i>Date: <span class="under">________</span>
          </div>
        </div>
      </div>
      <div class="card-body">
        <h3 class="card-title">
          Greetings from <span class="header-modifier">Argentina!</span>
        </h3>
        <p class="card-text">
          Since our travels began we have seen many amazing places around this
          beautiful country and eaten many delicous meals! A few of our
          favorites have been:
        </p>
        <ul class="activities">
          <a href="#"><li>Hiking in Patagonia</li></a>
          <a href="#"><li>Visiting the Tierra del Fuego archipeligos</li></a>
          <a href="#"><li>Wandering around Buenos Aires</li></a>
          <a href="#"><li>Eating asado barbecues</li></a>
        </ul>
        <div class="photo-link">
          <a href="#">View More Photos &rarr;</a>
        </div>
      </div>
    </div>
  </body>
</html>
```

```css
:root {
  --main-white: #f8f8f8;
  --main-gray-1: #dbe1e8;
  --secondary-gray: #b2becd;
  --secondary-gray-2: #6c7983;
  --dark-gray: #454e56;
  --main-black: #2a2e35;
  --secondary-black: #12181b;
  --accent-blue: #0084a5;
  --drop-down-custom: #add2eb;
  --main-bird: url('images/hummingbird.jpg');
  --night-bird: url('images/owl.jpg');

  --hue: 206;
  --accent-hue: 25;
  --text-color-dark-mode: hsl(var(--hue), 10%, 62%);
  --text-color-highlight: hsl(var(--accent-hue), 70%, 45%);
  --link-color: hsl(var(--hue), 90%, 70%);
  --accent-color: hsl(var(--accent-hue), 100%, 70%);
  --button-background: hsl(var(--hue), 63%, 43%);
  --dark-dropdown-background: hsl(var(--hue), 63%, 43%);
  --dark-mode-background: hsl(var(--hue), 20%, 12%);

  /* Custom theme variables go below */
}

.light {
  --bird: var(--main-bird);
  --bg: var(--main-white);
  --bg-nav: linear-gradient(
    to top,
    var(--main-gray-1),
    var(--secondary-gray-2)
  );
  --bg-dropdown: var(--main-white);
  --text: var(--secondary-black);
  --border-color: var(--accent-blue);
  --bg-custom: var(--drop-down-custom);
}

/* Dark theme code goes below */
.dark {
  --bird: var(--night-bird);
  --bg: var(--dark-mode-background);
  --bg-dropdown: var(--dark-dropdown-background);
  --text: var(--text-color-dark-mode);
  --border-color: var(--accent-color);
  --bg-custom: var(--drop-down-custom);
  --bg-nav: linear-gradient(to bottom, --link-color, --secondary-black);
}

/* Custom theme code goes below */

#dark::before {
  background: #2a2e35;
}

#light::before {
  background: #ffffff;
}

#custom-theme::before {
  background: var(--bg-custom);
}

*,
*::before,
*::after {
  box-sizing: border-box;
}

body {
  margin: 0;
  padding: 0;
  color: var(--text);
  background: var(--bg);
  transition: background 500ms ease-in-out, color 1000ms ease-in-out;
  font-family: 'Roboto', sans-serif;
}

ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
}

a {
  color: currentColor;
  text-decoration: none;
}

header {
  padding: 1em;
  background: var(--bg-nav);
  margin-bottom: 1em;
  padding-bottom: 2.5em;
  text-align: center;
}

main {
  max-height: 1000px;
  padding: 2rem;
}

.navbar {
  height: 75px;
  width: 100%;
  background: var(--bg);
  color: var(--text);
  font-size: clamp(0.8rem, 1rem, 1.2rem);
  font-weight: bold;
}

.navbar-nav {
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  height: 100%;
}

.dropdown {
  position: absolute;
  display: block;
  width: relative;
  opacity: 0;
  z-index: 2;
  background: var(--bg-dropdown);
  border-top: 2px solid var(--border-color);
  height: 3rem;
  margin-top: 2rem;
  padding: 0.5rem;
  filter: drop-shadow(30px 10px 6px rgba(2, 8, 20, 0.1));
  transform: translateX(-40%);
  transition: opacity 0.15s ease-out;
}

.has-dropdown:focus-within .dropdown {
  opacity: 1;
  pointer-events: auto;
  height: 125px;
  margin-top: 4px;
  width: 180px;
}

.dropdown-item a {
  width: 100%;
  height: 100%;
  size: 0.7rem;
  padding-left: 10px;
  font-weight: bold;
}

.dropdown-item a::before {
  content: ' ';
  border: 2px solid var(--border-color);
  border-radius: 50%;
  width: 2rem;
  height: 2rem;
  margin-top: 4px;
  display: inline-block;
  vertical-align: middle;
  margin-right: 10px;
}

img {
  display: block;
  border: 0;
  width: 100%;
  height: auto;
}

.container {
  display: flex;
  flex-direction: column;
  max-width: 100%;
  padding: calc(5% + 1.5vw);
}

.card {
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  margin: 0.5rem;
}

.center {
  display: flex;
  align-items: center;
  justify-content: center;
}

.header {
  background-image: url();
}

.subhead-1 {
  background-image: var(--bird);
}

.subhead-2 {
  background-image: url();
}

.block-1 {
  background-image: url();
}

.block-2 {
  background-image: url();
}

.block-3 {
  background-image: url();
}

.block-4 {
  background-image: url();
}

.block-5 {
  background-image: url();
}

.block-6 {
  background-image: url();
}

.block-7 {
  background-image: url();
}

@supports (display: grid) {
  @media screen and (min-width: 400px) {
    .card {
      margin: 0px;
    }

    .grid-wrapper {
      grid-template-columns: 1fr 1fr;
      grid-template-rows: 2fr 2fr 3fr 2fr 2fr 2fr;
      display: grid;
      height: 100em;
      justify-content: center;
      align-content: end;
      grid-gap: 7px;
    }

    .col-num {
      grid-column: span 2;
    }
  }

  @supports (display: grid) {
    @media screen and (min-width: 600px) {
      .grid-wrapper {
        grid-template-columns: repeat(6, 1fr);
      }

      .header {
        grid-column: span 3;
        grid-row-start: 2;
        grid-row-end: 4;
      }
      .subhead-1 {
        grid-column: span 3;
        grid-row-start: 1;
        grid-row-end: 2;
      }

      .subhead-2 {
        grid-column: span 3;
        grid-row-start: 1;
        grid-row-end: 3;
      }

      .block-1 {
        grid-column-start: 4;
        grid-column-end: 7;
        grid-row-start: 3;
        grid-row-end: 4;
      }

      .block-2 {
        grid-column: span 1;
        grid-row-start: 4;
        grid-row-end: 6;
      }

      .block-3 {
        grid-column-start: 2;
        grid-column-end: 4;
        grid-row-start: 4;
        grid-row-end: 6;
      }

      .block-4 {
        grid-column: span 2;
        grid-row-start: 4;
        grid-row-end: 5;
      }

      .block-5 {
        grid-column: span 2;
      }

      .block-6 {
        grid-column: span 1;
        grid-row-start: 4;
        grid-row-end: 5;
      }

      .block-7 {
        grid-column: span 1;
      }
    }
  }
}
```

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Curious Ornithologist</title>

    <link rel="preconnect" href="https://fonts.gstatic.com" />
    <link
      href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;700&display=swap"
      rel="stylesheet"
    />
    <link rel="stylesheet" href="style.css" />
    <script defer src="app.js"></script>
  </head>

  <body class="light">
    <!-- Navbar -->
    <nav class="navbar">
      <ul class="navbar-nav">
        <li class="nav-item">Home</li>
        <li class="nav-item">Bird Families</li>
        <li class="nav-item">About</li>

        <li class="nav-item has-dropdown">
          <a href="#">Theme</a>
          <ul class="dropdown">
            <li class="dropdown-item">
              <a id="light" href="#">Light Mode</a>
            </li>
            <li class="dropdown-item">
              <a id="dark" href="#">Dark Mode</a>
            </li>
            <li class="dropdown-item">
              <a id="custom-theme" href="#">Custom Mode</a>
            </li>
          </ul>
        </li>
      </ul>
    </nav>

    <header>
      <h1>A Curious Ornithologist</h1>

      <p>Interact with night and daytime birds using different page themes!</p>
    </header>

    <main>
      <div class="container">
        <div class="">
          <div id="grid" class="grid-wrapper">
            <div id="header-image" class="card header hero col-num"></div>
            <div class="card lrg subhead-1 col-num"></div>
            <div class="card lrg subhead-2 col-num"></div>
            <div class="card block-1"></div>
            <div class="card block-2"></div>
            <div class="card block-3"></div>
            <div class="card block-4"></div>
            <div class="card block-5"></div>
            <div class="card block-6"></div>
            <div class="card block-7"></div>
          </div>
        </div>
      </div>
    </main>
    <footer>copyright</footer>
  </body>
</html>
```

```js
// DOM Elements
const darkButton = document.getElementById('dark')
const lightButton = document.getElementById('light')
const customButton = document.getElementById('custom-theme')
const body = document.body

// Apply the cached theme on reload

const theme = localStorage.getItem('theme')
const isCustom = localStorage.getItem('isCustom')

if (theme) {
  body.classList.add(theme)
  isCustom && body.classList.add('custom-theme')
}

// Button Event Handlers

darkButton.onclick = () => {
  body.classList.replace('light', 'dark')
  body.classList.replace('custom-theme', 'dark')

  localStorage.setItem('theme', 'dark')
}

lightButton.onclick = () => {
  body.classList.replace('dark', 'light')
  body.classList.replace('custom-theme', 'light')
  localStorage.setItem('theme', 'light')
}

customButton.onclick = () => {
  body.classList.replace('dark', 'custom-theme')
  body.classList.replace('light', 'custom-theme')
  localStorage.setItem('theme', 'custom-theme')
}
```
