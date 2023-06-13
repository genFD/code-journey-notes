# VISUAL RULES

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)

  - [Font Family](#font-family)
  - [Font Size](#font-size)
  - [Font Weight](#font-weight)
  - [Text Align](#text-align)
  - [Color and Background Color](#color-and-background-color)
  - [Opacity](#opacity)
  - [Background Imagey](#background-image)
  - [Important](#important)

- [Selectors]

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

- Learn how to style individual and groups of elements using various visual CSS rules.

## Notes

_tldr_:

- The font-family property defines the typeface of an element.
- font-size controls the size of text displayed.
- font-weight defines how thin or thick text is displayed.
- The text-align property places text in the left, right, or center of its parent container.
- Text can have two different color attributes: color and background-color. color defines the color of the text, while background-color defines the color behind the text.
- CSS can make an element transparent with the opacity property.
- CSS can also set the background of an element to an image with the background-image property.
- The !important flag will override any style, however it should almost never be used, as it is extremely difficult to override.

### Font Family

**[⬆ back to top](#table-of-contents)**

To change the typeface of text on your web page, you can use the font-family property.

```css
h1 {
  font-family: Garamond;
}
```

### Font Size

**[⬆ back to top](#table-of-contents)**

To change the size of text on your web page, you can use the font-size property.

```css
p {
  font-size: 18px;
}
```

### Font Weight

**[⬆ back to top](#table-of-contents)**

the font-weight property controls how bold or thin text appears.

```css
p {
  font-weight: bold;
}
```

we could set the font weight of a particular element to normal, essentially shutting off bold for that element.

### Text Align

**[⬆ back to top](#table-of-contents)**

the text always appears on the left side of the container in which it resides.

To align text we can use the text-align property. The text-align property will align text to the element that holds it, otherwise known as its parent.

```css
h1 {
  text-align: right;
}
```

The text-align property can be set to one of the following commonly used values:

`left` — aligns text to the left side of its parent element, which in this case is the browser.
`center` — centers text inside of its parent element.
`right` — aligns text to the right side of its parent element.
`justify—` spaces out text in order to align with the right and left side of the parent element.

### Color and Background Color

**[⬆ back to top](#table-of-contents)**

Color can affect the following design aspects:

- Foreground color
- Background color

In CSS, these two design aspects can be styled with the following two properties:

- `color`: this property styles an element’s foreground color (the color that an element appears in)
- `background-color`: this property styles an element’s background color

```css
h1 {
  color: red;
  background-color: blue;
}
```

### Opacity

**[⬆ back to top](#table-of-contents)**

Opacity is the `measure of how transparent an element is`. It’s measured `from 0 to 1`, with `1 representing 100%`, or fully visible and opaque, and `0 representing 0%`, or fully invisible.

Opacity can be used to make elements fade into others for a nice overlay effect. To adjust the opacity of an element, the syntax looks like this:

```css
.overlay {
  opacity: 0.5;
}
```

the `.overlay` element would be 50% visible, letting whatever is positioned behind it show through.

### Background Image

**[⬆ back to top](#table-of-contents)**

The option to make the background of an element an image.

```css
.main-banner {
  background-image: url('https://www.example.com/image.jpg');
}
```

To link to an image inside an existing project, you must provide a relative file path. If there was an `image folder` in the project, with an image named `mountains.jpg`, the relative file path would look like :

```css
.main-banner {
  background-image: url('images/mountains.jpg');
}
```

### Important

**[⬆ back to top](#table-of-contents)**

`!important` can be applied to specific declarations, instead of full rules. It will `override any style no matter how specific it is`.

As a result, `it should almost never be used`. Once !important is used, it is very hard to override.

The syntax of !important in CSS looks like this:

```css
p {
  color: blue !important;
}

.main p {
  color: red;
}
```

`!important` is used on the p selector’s color attribute, all p elements will appear blue, even though there is a more specific .main p selector that sets the color attribute to red.
