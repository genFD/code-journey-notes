# Syntax and selectors

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)
- [Syntax]
  - [CSS Anatomy](#css-anatomy)
  - [Inline styles](#inline-styles)
  - [Internal Stylesheet](#internal-stylesheet)
  - [External Stylesheet](#external-stylesheet)
- [Selectors]

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

- Learn how to set up your CSS file structure
- Learn How to use selectors to apply styles to specific elements.

## Notes

### CSS Anatomy

**[⬆ back to top](#table-of-contents)**

![CSS anatomy](/notes/01-HTML-CSS/assets/CSS_Anatomy-v2-nobgfill.svg)

The diagram shows two different methods, or syntaxes, for writing CSS code.

- The first syntax shows CSS applied as a ruleset,
- while the second shows it written as an inline style.

**Terminology:**

`Ruleset Terms:`

- _Selector_ : The beginning of the ruleset used to target the element that will be styled.

- _Declaration Block_ : The code in-between (and including) the curly braces ({ }) that contains the CSS declaration(s).

- _Declaration_ : The group name for a property and value pair that applies a style to the selected element.

- _Property_ : The first part of the declaration that signifies what visual characteristic of the element is to be modified.

- _Value_ : The second part of the declaration that signifies the value of the property.

`Inline Terms:`

- _Opening Tag_ : The start of an HTML element. This is the element that will be styled.

- _Attribute_ : The style attribute is used to add CSS inline styles to an HTML element.

- _Declaration_ : The group name for a property and value pair that applies a style to the selected element.

- _Property_ : The first part of the declaration that signifies what visual characteristic of the element is to be modified.

- _Value_ : The second part of the declaration that signifies the value of the propert.

### Inline styles

**[⬆ back to top](#table-of-contents)**

To style an HTML element,

- you can add the style attribute directly to the opening tag. After you add the attribute, you can set it equal to the CSS style(s) you’d like applied to that element.

```html
<p style="color: red;">I'm learning to code!</p>
```

- The paragraph element has a style attribute within its opening tag

- the style attribute is set equal to color: red which will set the color of the paragraph text to red within the browser.

If you’d like to add more than one style with inline styles, simply keep adding to the style attribute. Make sure to end the styles with a semicolon (;)

```html
<p style="color: red; font-size: 20px;">I'm learning to code!</p>
```

**It’s important to know that inline styles are a quick way of directly styling an HTML element, but are rarely used when creating websites.**

### Internal Stylesheet

**[⬆ back to top](#table-of-contents)**

inline styles are not the best way to style HTML elements because :

- If you wanted to style, for example, multiple `<h1>` elements, you would have to add inline styling to each element manually.

Fortunately, it's possible to `write CSS code with a <style> element nested inside of the <head> element`. The CSS code inside the `<style>` element is often referred to as an `internal stylesheet`.

To create an internal stylesheet, a `<style>` element must be placed inside of the `<head>` element.

```html
<head>
  <style></style>
</head>
```

After adding opening and closing `<style>` tags in the head section, you can begin writing CSS code.

```html
<head>
  <style>
    p {
      color: red;
      font-size: 20px;
    }
  </style>
</head>
```

### External Stylesheet

**[⬆ back to top](#table-of-contents)**

Because of SOC, it's recommended to avoid mixing HTML (the structure) and CSS code(styling).
You can create an external stylesheet by using the `.css file` name extension, like so: `style.css`.

When HTML and CSS codes are in separate files, the files must be linked.
You can use the `<link>` element to link HTML and CSS files together.
The `<link>` element must be placed within the head of the HTML file.
It is a self-closing tag and requires the following attributes:

- `href` — like the anchor element, the value of this attribute must be the address, or path, to the CSS file.

- `rel` — this attribute describes the relationship between the HTML file and the CSS file. Because you are linking to a stylesheet, the value should be set to stylesheet

`url`

```html
<link href="/stylesheets.com/style.css" rel="stylesheet" />
```

`relative path`

```html
<link href="./style.css" rel="stylesheet" />
```

[Selectors]

_tldr_:

- CSS can select HTML elements by type, class, ID, and attribute.
- All elements can be selected using the universal selector.
- An element can have different states using the pseudo-class selector.
- Multiple CSS classes can be applied to one HTML element.
- Classes can be reusable, while IDs can only be used once.
- IDs are more specific than classes, and classes are more specific than type. That means IDs will override any styles from a class, and classes will override any styles from a type selector.
- Multiple selectors can be chained together to select an element. This raises the specificity but can be necessary.
- Nested elements can be selected by separating selectors with a space.
- Multiple unrelated selectors can receive the same styles by separating the selector names with commas.

### Type

**[⬆ back to top](#table-of-contents)**

- How do you decide which elements will get the style? With a selector.

- A selector is used to `target the specific HTML element(s) to be styled by the declaration`

- the type selector matches the type of the element in the HTML document.

```css
p {
  color: green;
}
```

The element type is p, which comes from the HTML `<p>` element.

### Universal

**[⬆ back to top](#table-of-contents)**

- the universal selector selects all elements of any type.
- there are couple of use cases, such as resetting default browser styling, or selecting all children of a parent element
- The universal selector uses the `*` character

```css
* {
  font-family: Verdana;
}
```

- every text element on the page will have its font changed to `Verdana`.

### Class

**[⬆ back to top](#table-of-contents)**

- CSS is not limited to selecting elements by their type. As you know, HTML elements can also have attributes

- a class attribute is one of the most common ways to select an element.

For example, consider the following HTML:

```html
<p class="brand">Sole Shoe Company</p>
```

The paragraph element in the example above has a class attribute set to `brand`.

- To select this element using CSS, we can create a ruleset with a class selector of `.brand`

```css
.brand {
}
```

### Multiple Classes

**[⬆ back to top](#table-of-contents)**

- it’s possible to add more than one class name to an HTML element’s class attribute

- For instance, perhaps there’s a heading element that needs to be green and bold. You could write two CSS rulesets like so:

```css
.green {
  color: green;
}

.bold {
  font-weight: bold;
}
```

Then, you could include both of these classes on one HTML element like this:

```html
<h1 class="green bold">...</h1>
```

### ID

**[⬆ back to top](#table-of-contents)**

- When it’s important to select a single element with CSS to give it its own unique style, we can give it an `ID` using the id attribute.

```html
<h1 id="large-title">...</h1>
```

- In contrast to class which accepts multiple values, and can be used broadly throughout an HTML document, an element’s id can only have a single value, and only be used once per page.

- To select an element’s ID with CSS, we prepend the id name with a number sign (#)

```css
#large-title {
}
```

### Attribute

**[⬆ back to top](#table-of-contents)**

- The attribute selector can be used to target HTML elements that already contain attributes
- This alleviates the need to add new code, like the class or id attributes.
- Attributes can be selected similarly to types, classes, and IDs.

```css
[href] {
  color: magenta;
}
```

- The most basic syntax is an attribute surrounded by square brackets. In the above example: [href] would target all elements with an href attribute and set the color to magenta.

- And it can get more granular. One way is by using type[attribute*=value]. this code selects an element where the attribute contains any instance of the specified value

```html
<img src="/images/seasons/cold/winter.jpg" />
<img src="/images/seasons/warm/summer.jpg" />
```

- The HTML code above renders two `<img>` elements, each containing a src attribute with a value equaling a link to an image file.

```css
img[src*='winter'] {
  height: 50px;
}

img[src*='summer'] {
  height: 100px;
}
```

- The first ruleset looks for an img element with an attribute of src that contains the string 'winter', and sets the height to 50px

- The second ruleset looks for an img element with an attribute of src that contains the string 'summer', and sets the height to 100px.

- no new HTML markup (like a class or id) needed to be added, and we were still able to modify the styles of each image independently. This is one advantage to using the attribute selector!

### Pseudo-class

**[⬆ back to top](#table-of-contents)**

- The appearance of certain elements can change, or be in a different state, after certain user interactions. For instance:

1. When you click on an`<input>` element, and a blue border is added showing that `it is in focus`.
2. When you click on a blue `<a>` link to visit to another page, but when you return the link’s text is purple.
3. When you’re filling out a form and the submit button is grayed out and disabled. But when all of the fields have been filled out, the button has color showing that it’s active.

- These are all examples of pseudo-class selectors in action! In fact, `:focus`, `:visited`, `:disabled`, and `:active` are all pseudo-classes.

- Factors such as user interaction, site navigation, and position in the document tree can all give elements a different state with pseudo-class.

- A pseudo-class can be attached to any selector. It is always written as a colon : followed by a name

```css
p:hover {
  background-color: lime;
}
```

In the code above whenever the `mouse hovers over a paragraph element`, that paragraph will have a `lime-colored background`

### Classes and IDs

**[⬆ back to top](#table-of-contents)**

- CSS classes are meant to be reused over many elements.
- By writing CSS classes, you can style elements in a variety of ways by mixing classes
- For instance, imagine a page with two headlines. One headline needs to be bold and blue, and the other needs to be bold and green. Instead of writing separate CSS rules for each headline that repeat each other’s code, it’s better to write a `.bold` CSS rule, a `.green` CSS rule, and a `.blue` CSS rule. Then you can give one headline the bold green classes, and the other the bold blue classes.

```html
<h1 class="bold green"></h1>
<h2 class="bold blue"></h2>
```

We've used the bold class two times.

- an ID is meant to style only one element
- IDs override the styles of types and classes. Since IDs override these styles, they should be used sparingly and only on elements that need to always appear the same

### Specificity

**[⬆ back to top](#table-of-contents)**

- Specificity is `the order by which the browser decides which CSS styles will be displayed`

- Best practice: use the lowest degree of specificity so that if an element needs a new style, it is easy to override.

1. IDs are the most specific selector in CSS (id wins the specificity war!!)
2. followed by classes,
3. and finally, type

for example

```html
<h1 class="headline">Breaking News</h1>
```

```css
h1 {
  color: red;
}

.headline {
  color: firebrick;
}
```

- In the example code above, the color of the heading would be set to firebrick, because the class selector is more specific than the type selector. If an ID was added, the styles within the ID selector’s body would override all other styles for the heading.

- To make styles easy to edit, it’s best to style with a type selector, if possible. If not, add a class selector. If that is not specific enough, then consider using an ID selector.

### Chaining

**[⬆ back to top](#table-of-contents)**

- It's possible to combine multiple selectors, which we refer to as chaining.

```css
h1.special {
}
```

- The code above would select only the `<h1>` elements with a class of special. If a `<p>`element also had a class of special, the rule in the example would not style the paragraph

- Selecting only the `<h2>` elements with class destination

```css
h2.destination {
  font-family: Tahoma;
}
```

### Descendant Combinator

**[⬆ back to top](#table-of-contents)**

- It's also possible to select elements that are nested within other HTML elements, also known as descendants

```html
<ul class="main-list">
  <li>...</li>
  <li>...</li>
  <li>...</li>
</ul>
```

The nested `<li>` elements are descendants of the `<ul>` element and can be selected with the descendant combinator like so:

```css
.main-list li {
}
```

instead of selecting all `<li>` elements, we selected only the `<li>` elements nested inside the `.main-list` elements

Adding more than one tag, class, or ID to a CSS selector increases the specificity of the CSS selector.

### Chaining and specifity

**[⬆ back to top](#table-of-contents)**

```css
p {
  color: blue;
}

.main p {
  color: red;
}
```

- Both of these CSS rules define what a `<p>` element should look like.

- Since `.main p` has a class and a `p` type as its selector, only the `<p>` elements inside the `.main` element will appear red.

### Multiple Selectors

**[⬆ back to top](#table-of-contents)**

- it’s possible to add CSS styles to multiple CSS selectors all at once.This prevents writing repetitive code.

```css
h1 {
  font-family: Georgia;
}

.menu {
  font-family: Georgia;
}
```

- Instead of writing font-family: Georgia twice for two selectors, we can separate the selectors by a comma to apply the same style to both, like this:

```css
h1,
.menu {
  font-family: Georgia;
}
```
