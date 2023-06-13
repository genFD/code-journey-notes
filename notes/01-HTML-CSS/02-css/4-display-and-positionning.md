# DISPLAY AND POSITIONING

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

- Learn CSS rules for displaying and positioning elements on your web page.
- Learn five properties for adjusting the position of HTML elements in the browser

## Notes

_tldr_:

- The position property allows you to specify the position of an element. When set to relative, an element’s position is relative to its default position on the page.

- When set to absolute, an element’s position is relative to its closest positioned parent element. It can be pinned to any part of the web page, but the element will still move with the rest of the document when the page is scrolled.

- When set to fixed, an element’s position can be pinned to any part of the web page. The element will remain in view no matter what.

- When set to sticky, an element can stick to a defined offset position when the user scrolls its parent container.

- The z-index of an element specifies how far back or how far forward an element appears on the page when it overlaps other elements.The display property allows you to control how an element flows vertically and horizontally in a document.

- Inline elements take up as little space as possible, and they cannot have manually adjusted width or height.

- Block elements take up the width of their container and can have manually adjusted heights.

- Inline-block elements can have set width and height, but they can also appear next to each other and do not take up their entire container width.
- The float property can move elements as far left or as far right as possible on a web page.

- You can clear an element’s left or right side (or both) using the clear property.

### Flow of HTML

**[⬆ back to top](#table-of-contents)**

A browser will render the elements of an HTML document that has no CSS from left to right, top to bottom, in the same order as they exist in the document. This is called the flow of elements in HTML.

In addition to the properties that it provides to style HTML elements, CSS includes properties that change how a browser positions elements. These properties specify where an element is located on a page, if the element can share lines with other elements, and other related attributes.

In this lesson, you will learn five properties for adjusting the position of HTML elements in the browser:

- Position
- Display
- Z-index
- Float
- Clear

Each of these properties will allow us to position and view elements on a web page

### Position

**[⬆ back to top](#table-of-contents)**

![Position ](/Notes/notes/01-HTML-CSS/assets/Position-updated.webp)

Block-level elements like these boxes create a block the full width of their parent elements, and they prevent other elements from appearing in the same horizontal space.

Notice the block-level elements in the image above take up their own line of space and therefore don’t overlap each other.

This is the default position for block-level elements.

The default position of an element can be changed by setting its position property.
The position property can take one of five values:

`static` - the default value (it does not need to be specified)
`relative`
`absolute`
`fixed`
`sticky`

### Position: Relative

**[⬆ back to top](#table-of-contents)**

One way to modify the default position of an element is by setting its position property to `relative`.

This value allows you to `position an element relative to its default static position on the web page`.

```css
.green-box {
  background-color: green;
  position: relative;
}
```

The code in the example above does not specify where the `.green-box` element should be positioned on the page. This is done by accompanying the position declaration with one or more of the following offset properties that will move the element away from its default static position:

`top` - moves the element down from the top.
`bottom` - moves the element up from the bottom.
`left` - moves the element away from the left side (to the right).
`right` - moves the element away from the right side (to the left).

You can specify values in pixels, ems, or percentages, among others, to dial in exactly how far you need the element to move. It’s also important to note that offset properties will not work if the element’s position property is the default static.

```css
.green-box {
  background-color: green;
  position: relative;
  top: 50px;
  left: 120px;
}
```

In the example above, the element of green-box class will be moved down 50 pixels, and to the right 120 pixels, from its default static position.

![Position - Relative](/Notes/notes/01-HTML-CSS/assets/Relative.webp)

### Position: Absolute

**[⬆ back to top](#table-of-contents)**

When an element’s position is set to absolute, all other elements on the page will ignore the element and act like it is not present on the page.
position an element element?
The element will be positioned in relation to the nearest non-static parent element, while offset properties can be used to determine the final position from there.

![Position - Relative](/Notes/notes/01-HTML-CSS/assets/position-absolute.webp)

The `“This website is in progress. Please come back later!”` text is displaced from its static position at the top left corner of its parent container. It has offset property declarations of top: 300px; and right: 0px;, positioning it 300 pixels down, and 0 pixels from the right side of the page

### Position: Fixed

**[⬆ back to top](#table-of-contents)**

When an element’s position is set to absolute, as in the last exercise, `the element will scroll with the rest of the document when a user scrolls`.

We can fix an element to a specific position on the page (regardless of user scrolling) by setting its position to fixed, and accompanying it with the familiar offset properties top, bottom, left, and right.

```css
.title {
  position: fixed;
  top: 0px;
  left: 0px;
}
```

In the example above, the .title element will remain fixed to its position no matter where the user scrolls on the page, like in the image below:

![Position - Fixed](/Notes/notes/01-HTML-CSS/assets/Fixed.gif)

This technique is often used for navigation bars on a web page.

### Position: Sticky

**[⬆ back to top](#table-of-contents)**

Since static and relative positioned elements stay in the normal flow of the document, when a user scrolls the page (or parent element) these elements will scroll too. And since fixed and absolute positioned elements are removed from the document flow, when a user scrolls, these elements will stay at their specified offset position.

The sticky value is another position value that keeps an element in the document flow as the user scrolls, but sticks to a specified position as the page is scrolled further. This is done by using the sticky value along with the familiar offset properties, as well as one new one.

```css
.box-bottom {
  background-color: darkgreen;
  position: sticky;
  top: 240px;
}
```

![Position - Static](/Notes/notes/01-HTML-CSS/assets/Sticky.gif)

In the example above, the .box-bottom <div> will remain in its relative position, and scroll as usual. When it reaches 240 pixels from the top, it will stick to that position until it reaches the bottom of its parent container where it will “unstick” and rejoin the flow of the document.

### Z-INDEX

**[⬆ back to top](#table-of-contents)**

When boxes on a web page have a combination of different positions, the boxes (and therefore, their content) can overlap with each other, making the content difficult to read or consume.

```css
.blue-box {
  background-color: blue;
}

.green-box {
  background-color: green;
  position: relative;
  top: -170px;
  left: 170px;
}
```

The `z-index` property controls `how far back or how far forward an element should appear on the web page` when elements overlap. This can be thought of as the depth of elements, with deeper elements appearing behind shallower elements.

The `z-index` property accepts integer values.

```css
.blue-box {
  background-color: blue;
  position: relative;
  z-index: 1;
}

.green-box {
  background-color: green;
  position: relative;
  top: -170px;
  left: 170px;
}
```

In the example above, we set the `.blue-box` position to relative and the z-index to 1. We changed position to relative, because `the z-index property does not work on static elements`.

The z-index of 1 moves the `.blue-box` element forward, because the `z-index` value has not been explicitly specified for the .green-box element, which means it has a default `z-index`value of 0.

![Position - Z-index](/Notes/notes/01-HTML-CSS/assets/Z-index.webp)

### Inline Display

**[⬆ back to top](#table-of-contents)**

Every HTML element has a default display value that dictates `if it can share horizontal space with other elements`

The default display for some elements, such as `<em>,` `<strong>`, and `<a>`, is called inline.

Inline elements have a box that :

- wraps tightly around their content,
- only taking up the amount of space necessary to display their content
- and not requiring a new line after each element.

`The height and width of these elements cannot be specified in the CSS document`.

```html
To learn more about <em>inline</em> elements, read
<a href="#">MDN documentation</a>.
```

The `CSS display property` provides the ability to make any element an inline element. This includes elements that are not inline by default such as paragraphs, divs, and headings.

```css
h1 {
  display: inline;
}
```

The CSS in the example above will change the display of all `<h1>` elements to inline. The browser will render `<h1>` elements on the same line as other inline elements immediately before or after them (if there are any).

**An inline element does not start a new line and cannot be sized using the height and width properties.**

### Display: Block

**[⬆ back to top](#table-of-contents)**

Some elements are not displayed in the same line as the content around them. These are called block-level elements.

These elements :

- fill the entire width of the page by default, but their width property can also be set.
- they are the height necessary to accommodate their content.

Elements that are block-level by default include all levels of heading elements (`<h1>` through `<h6>`), `<p>`, `<div>` and `<footer>`.

### Display: Inline-Block

**[⬆ back to top](#table-of-contents)**

The third value for the display property is `inline-block`.
Inline-block display combines features of both inline and block elements
Inline-block elements can :

- appear next to each other
- we can specify their dimensions using the width and height

For example, the `<div>s` below will be displayed on the same line and with the specified dimension:

![Inline-block](/Notes/notes/01-HTML-CSS/assets/display-inline-block.webp)

Let’s take a look at the code :

```html
<div class="rectangle">
  <p>I’m a rectangle!</p>
</div>
<div class="rectangle">
  <p>So am I!</p>
</div>
<div class="rectangle">
  <p>Me three!</p>
</div>
```

```css
.rectangle {
  display: inline-block;
  width: 200px;
  height: 300px;
}
```

There are three rectangular `divs` that each contain a paragraph of text. The .rectangle `<div>s` will all appear inline (provided there is enough space from left to right) with a width of 200 pixels and height of 300 pixels, even though the text inside of them may not require 200 pixels by 300 pixels of space.

**An inline-block element does not start a new line and can be sized using the height and width properties.**

### Float

**[⬆ back to top](#table-of-contents)**

The float property is commonly used for wrapping text around an image.
The float property is often set using one of the values below:

`left` - moves, or floats, elements as far left as possible.
`right` - moves elements as far right as possible.

```css
.green-section {
  width: 50%;
  height: 150px;
}

.orange-section {
  background-color: orange;
  width: 50%;
  float: right;
}
```

In the example above, we float the `.orange-section` element to the right. This works for static and relative positioned elements

Floated elements must have a width specified, as in the example above. Otherwise,

- the element will assume the full width of its containing element,
- and changing the float value will not yield any visible results.

### Clear

**[⬆ back to top](#table-of-contents)**

The clear property specifies how elements should behave when they bump into each other on the page. It can take on one of the following values:

`left` — the left side of the element will not touch any other element within the same containing element.

`right`— the right side of the element will not touch any other element within the same containing element.

`both` — neither side of the element will touch any other element within the same containing element.

`none`— the element can touch either side

```css
div {
  width: 200px;
  float: left;
}

div.special {
  clear: left;
}
```

In the example above, all `<div>s`on the page are floated to the left side. The element with class special did not move all the way to the left because a taller `<div>`blocked its positioning. By setting its clear property to left, the special `<div>` will be moved all the way to the left side of the page.
