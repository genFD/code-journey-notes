# VARIABLES

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)
  - [Defining Variables](#defining-variables)
  - [Using Variables](#using-variables)
  - [Scoping Variables](#scoping-variables)
  - [Inheriting and Overriding Variables](#scoping-variables)
  - [Fallback Values](#fallback-values)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

- Learn how to use CSS variables and functions to create more organized, efficient, and dynamic websites.

## Notes

_tldr_:

- Variables mitigate the need to repeat property values and make CSS code easier to read.
- You can use variables in CSS to store values.
- Variable declaration must start with a double hyphen (--).
- Variables must be used as values for CSS properties.
- Variables must be used as an argument inside of the var() function.
- Variables are subject to both scope and inheritance.
- Globally scoped variables are defined inside the :root pseudo-class.
- Overriding a variable is done by redefining that variable’s value inside of the desired selector ruleset.
- Fallback values can be used to provide a backup value if the initial variable is invalid.
- Multiple fallback values can be provided by adding more values inside cascading var() functions.
- Responsively designed web pages can be created by combining variables with media queries.

### Intro

Variables allow information to be maintained and referenced in multiple locations. They also make it easier to read and understand code. Especially when it’s someone else’s!

- CSS has variables in its own style—more specifically, they are called `Custom Properties`.

- These variables can be used to `hold specific values that can be reused throughout the stylesheet`.

- Variables `mitigate the need to repeat values` and `make values easier to understand` what they are used for.

- `--main-red-color` is much easier to understand than #7f3f3f.

#### Defining Variables

**[⬆ back to top](#table-of-contents)**

- Each variable declaration must begin with a double hyphen `(--)` followed by the variable name.

- After the variable name is declared a value can then be assigned to it.

```css
h1 {
  --main-header-color: #dadecc;
}
```

- we have a variable named `--main-header-color`
- with a color value `#DADECC` assigned to it.

There are specific naming conventions to keep in mind when declaring variables :

1. variables are case sensitive, meaning `--body-text-color` and `--Body-Text-Color` are two different variables.

2. It is also good practice to avoid using capital letters in variable names to prevent this type of confusion

3. it is common to use hyphen delimited strings such as writing `--list-background-color` instead of `--listBackgroundColor`

#### Using Variables

**[⬆ back to top](#table-of-contents)**

- To use CSS variables as values of properties, we must specify the variable’s name as an argument inside of the `var() function.`

```css
h1 {
  --main-background-color: #dadecc;
  background-color: var(--main-background-color);
}
```

- To set the background color of the h1 selector, the `-main-background-color` variable was declared and given a value of #DADECC.

Then on the next line, we use the variable as the value of the background-color

- `by passing --main-background-color as the argument of the var() function`.

Another powerful way to use CSS variables is

- to create variables based on other variables.
- This can be useful for `keeping track of color schemes` and `to keep CSS rules modular`.

```css
html {
  --custom-purple: #ff64ed;
  --main-color: var(--custom-purple);
}

body {
  background: var(--main-color);
}
```

- we first declared a variable called `--custom-purple` and gave it a value of `#FF64ED`

- Then, we `defined a variable called --main-color` and `set its value to be the --custom-purple variable` defined above it.

- Now, instead of referring to `--custom-purple` in the html and body selectors

- we are able to refer to `--main-color` to set the background color.

#### Scoping Variables

**[⬆ back to top](#table-of-contents)**

- When we define a CSS variable, we are also giving that variable a set scope.

- Variables can have two kinds of scope: local and global. So far we have only dealt with variables with local scope

- A locally scoped CSS variable will only affect `the specific HTML element that it is declared in along with any children that element may contain`.

```html
<nav id="menu-items">
  <ul>
    <li><a href="#">One</a></li>
    <li><a href="#">Two</a></li>
    <li><a href="#">Three</a></li>
  </ul>
</nav>
```

- the `<nav>`element with the id of `'menu-items'` contains an unordered list.

```css
#menu-items {
  --menu-color-blue: blue;
}

#menu-items a {
  color: var(--menu-color-blue);
}
```

Because the `--menu-color-blue` variable was declared inside the `#menu-items selector`,

- only `#menu-items` and `its children` can reference the variable.

Globally scoped variables are declared in the `:root`pseudo-class.

- This pseudo-class points to the root element of the document.

- In most cases that root element is actually the `<html>`element.

- By declaring variables in `:root` they `can be applied globally across the entire HTML document`

```css
:root {
  --menu-color-blue: blue;
}

#menu-items a {
  color: var(--menu-color-blue);
}
```

- It is common practice to define variables inside the `:root selector` but not mandatory.

#### Inheriting and Overriding Variables

**[⬆ back to top](#table-of-contents)**

```css
:root {
  --orange-color: #ff933a;
}

body {
  background-color: var(--orange-color);
}

button {
  --orange-color: #bf6317;
  color: var(--orange-color);
  border: 1px solid black;
}
```

- We re-declare an `--orange-color` variable inside of a specific button selector.
- And when that `--orange-color` variable is referenced in the specific selector it will be the locally chosen orange color.

#### Fallback Values

**[⬆ back to top](#table-of-contents)**

- Fallback values can be provided as the second and optional argument of the var() function.

- they will be used if the variable given as the first argument is invalid.

```css
body {
  background: var(--main-background-color, #f3f3f3);
}
```

- If a value of `--main-background-color` hasn’t been explicitly defined in the stylesheet or returns a non-color value, then the fallback value of `#F3F3F3` is used.

- The fallback value may also be a CSS variable, in which case it must be passed using another var() function.

- note that the var() function accepts a maximum of two arguments.

```css
body {
  /* --favorite-orange if --main-color is invalid and red if --favorite-orange is invalid */
  font-color: var(--main-color, var(--favorite-orange, red));
}
```

- we set `--favorite-orange` as the fallback value of the `--main-color` variable.

- red as the fallback value of the `--favorite-orange` variable.

#### Responsiveness

**[⬆ back to top](#table-of-contents)**

variables are also extremely powerful when used with media queries to create responsive designs

- We can dynamically change styles according to changes in viewport size and general system preference.

For example,

- We can change the color scheme of our website when a user has their system preference set for dark themes versus light themes.

```css
@media screen and (min-width: 600px) {
  :root {
    /* Light Color Theme */
    --body-background: lightblue;
    --inner-margin: 6px;
    --body-text-color: black;
    --font-size: 18px;
  }
}

@media screen and (max-width: 600px) {
  :root {
    /* Dark Color Theme */
    --body-background: #000;
    --inner-margin: 12px;
    --body-text-color: #fff;
    --font-size: 12px;
  }
}
```

- We are changing the general style and font size of the web page depending on the size of the page.

- If the screen width is 600px or smaller, styles for a dark theme are applied.

- If the screen size is larger than 600px, then it switches to a light theme
