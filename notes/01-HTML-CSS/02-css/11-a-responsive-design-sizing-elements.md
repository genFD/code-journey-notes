# RESPONSIVE DESIGN - SIZING ELEMENTS

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)
- [Sizing elements](#sizing-elements)
  - [Relative Measurements](#relative-measurements)
  - [EM](#em)
  - [REM](#rem)
  - [Percentages: Height & Width](#percentages-height--width)
  - [Percentages: Padding & Margin](#percentages-padding--margin)
  - [Width: Minimum & Maximum](#width-minimum--maximum)
  - [Height: Minimum & Maximum](#height-minimum--maximum)
  - [Scaling Background Images](#scaling-background-images)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

- Learn responsive design techniques, like relative sizing units and media queries, to create websites viewable on devices of all sizes.

## Notes

_tldr_:

- Content on a website can be sized relative to other elements on the page using relative measurements.

- The unit of `em` sizes font relative to the font size of a parent element.

- The unit of `rem` sizes font relative to the font size of a root element. That root element is the `<html>` element.

- Percentages are commonly used to size box-model features, like the width, height, padding, or margin of an element.

- When percentages are used to size width and height, child elements will be sized relative to the dimensions of their parent (remember that parent dimensions must first be set).

- Percentages can be used to set padding and margin. Horizontal and vertical padding and margin are set relative to the width of a parent element.

- The minimum and maximum width of elements can be set using min-width and max-width.

- The minimum and maximum height of elements can be set using min-height and max-height.

- When the height of an image or video is set, then its width can be set to auto so that the media scales proportionally. Reversing these two properties and values will also achieve the same result.

- A background image of an HTML element will scale proportionally when its background-size property is set to cover.

### Sizing elements

#### Relative Measurements

**[⬆ back to top](#table-of-contents)**

- `Premise` : Modern technology allows users to browse the Internet via multiple devices, such as desktop monitors, mobile phones, tablets, and more.

- `Problem`: Devices of different screen sizes, however, pose a problem for web developers: `how can we ensure that a website is readable and visually appealing across all devices, regardless of screen size?`

- The answer: `responsive design!` Responsive design refers to the ability of a website to resize and reorganize its content based on :

1. The size of other content on the website.
2. The size of the screen the website is being viewed on.

We will learn how to size HTML content relative to other content on a website.

- Pixels are fixed, hard coded values. When a screen size changes (like switching from landscape to portrait view on a phone), elements sized with pixels can appear too small, overflow the screen, or become completely illegible.

- you can avoid hard coded measurements and use relative measurements instead. Relative measurements offer an advantage over hard coded measurements, as `they allow for the proportions of a website to remain intact regardless of screen size or layout`.

#### EM

**[⬆ back to top](#table-of-contents)**

- Incorporating relative sizing starts by using units other than pixels
- One unit of measurement you can use in CSS to create relatively-sized content is the `em`, written as `em` in CSS.

em represents :

- the font-size of the current element
- or the default base font-size set by the browser if none is given.

For example, if the base font of a browser is 16 pixels (which is normally the default size of text in a browser),

- then `1 em is equal to 16 pixels`. `2 ems would equal 32 pixels`, and so on.

```css
heading {
  font-size: 2em;
}
```

In the example above :

- no base font has been specified
- therefore the font size of the heading element will be set relative to the default font size of the browser.
- Assuming the default font size is 16 pixels, then the font size of the heading element will be 32 pixels.

```css
.splash-section {
  font-size: 18px;
}

.splash-section h1 {
  font-size: 1.5em;
}
```

- a base font size (18px) is defined for all text within the splash-section element.
- The second CSS rule will set the font size of all h1 elements inside of splash-section relative to the base font of splash-section (18 pixels).
- The resulting font size of h1 elements will be 27 pixels.

#### REM

**[⬆ back to top](#table-of-contents)**

The second relative unit of measurement in CSS is the `rem`, coded as `rem`.

- Rem stands for root em. It acts similar to em, but instead of checking parent elements to size font, it checks the root element. The root element is the `<html>`tag.

- Most browsers set the font size of `<html>` to 16 pixels, so by default rem measurements will be compared to that value.

- To set a different font size for the root element, you can add a CSS rule.

```css
html {
  font-size: 20px;
}

h1 {
  font-size: 2rem;
}
```

- the font size of the root element, `<html>`, is set to 20 pixels.
- the size of h1 elements in the example will be 40 pixels.

One advantage of using rems is that :

- all elements are compared to the same font size value, making it easy to predict how large or small font will appear.

1. If you are interested in sizing elements `consistently across an entire website` : `the rem` measurement is the best unit for the job.
2. If you’re interested in sizing elements `in comparison to other elements nearby` : then the `em unit` would be better suited for the job.

#### Percentages: Height & Width

**[⬆ back to top](#table-of-contents)**

- to size `non-text HTML elements relative to their parent elements on the page you can use percentages`.

- Percentages are often used to size box-model values :

1. width and height
2. padding
3. border
4. margins

- They can also be used to set positioning properties (top, bottom, left, right).

```css
.main {
  height: 300px;
  width: 500px;
}

.main .subsection {
  height: 50%;
  width: 50%;
}
```

- `.main` and `.subsection` each represent divs.

- `.subsection` div is nested within the .main div.

- `.main` is 500px wide and 300px tall.

- When percentages are used, elements are sized relative to the dimensions of their parent element (also known as a container)

- Therefore, the dimensions of the `.subsection div` will be `150 pixels tall`, and `250 pixels wide`

- Be careful, a child element’s dimensions may be set erroneously if the dimensions of its parent element aren’t set first.

- While tempting, `100%` should only be used `when content will not have padding, border, or margin`. why ? Because the box model includes padding, borders, and margins, setting an element’s width to 100% may cause content to overflow its parent container.

#### Percentages: Padding & Margin

**[⬆ back to top](#table-of-contents)**

- Percentages can also be used to set the padding and margin of elements.

- When height and width are set using percentages, the dimensions of child elements are calculated based on the dimensions of the parent element.

- However, when percentages are used to set padding and margin : `they are calculated based only on the width of the parent element`.

For example :

- When a property like margin-left is set using a percentage (say 50%).

- the element will be moved halfway to the right in the parent container.

- as opposed to the child element receiving a margin half of its parent’s margin

Vertical padding and margin are also calculated based on the width of the parent. Why? Consider the following scenario:

1. A container div is defined, but its height is not set (meaning it’s flat)
2. The container then has a child element added within. The child element does have a set height. This causes the height of its parent container to stretch to that height.
3. The child element requires a change, and its height is modified. This causes the parent container’s height to also stretch to the new height.
4. This cycle occurs endlessly whenever the child element’s height is changed!

`an unset height (the parent’s) results in a constantly changing height due to changes to the child element.`

This is why vertical padding and margin are based on the width of the parent, and not the height.

**Note: When using relative sizing, ems and rems should be used to size text and dimensions on the page related to text size (i.e. padding around text). This creates a consistent layout based on text size. Otherwise, percentages should be used**.

```css
html {
  font-size: 16px;
}

#banner {
  height: 46rem;
}
```

- The root element’s font size is 16 pixels, meaning that 46rem will result in a height of 736 pixels.

```css
#blog {
  width: 86%;
}

#blog .post {
  width: 52%;
  margin-top: 12.5%;
  margin-bottom: 7.5%;
}
```

#### Width: Minimum & Maximum

**[⬆ back to top](#table-of-contents)**

Although relative measurements provide consistent layouts across devices of different screen sizes:

- elements on a website can lose their integrity when they become too small or large.

- You can limit how wide an element becomes with the following properties:

1. `min-width` — ensures a minimum width for an element.
2. `max-width` — ensures a maximum width for an element.

```css
p {
  min-width: 300px;
  max-width: 600px;
}
```

When a browser window is `narrowed or widened,` text can become either very compressed or very spread out, making it difficult to read.

- when the browser is resized, the width of paragraph elements will not fall below 300 pixels,
- nor will their width exceed 600 pixels

These two properties ensure that content is legible by limiting the minimum and maximum widths.

**Note:The unit of pixels is used to ensure hard limits on the dimensions of the element(s).**

#### Height: Minimum & Maximum

**[⬆ back to top](#table-of-contents)**

You can also limit the minimum and maximum height of an element.

1. `min-height` — ensures a minimum height for an element’s box.
2. `max-height` — ensures a maximum height for an element’s box.

```css
p {
  min-height: 150px;
  max-height: 300px;
}
```

- the height of all paragraphs will not shrink below 150 pixels and the height will not exceed 300 pixels.

#### Scaling Images and Videos

**[⬆ back to top](#table-of-contents)**

When a website contains such media, it’s important to make sure that it is scaled proportionally so that users can correctly view it.

```css
.container {
  width: 50%;
  height: 200px;
  overflow: hidden;
}

.container img {
  max-width: 100%;
  height: auto;
  display: block;
}
```

- .container represents `a container div.` It is set to a width of 50% (half of the browser’s width, in this example) and a height of 200 pixels.

- Setting overflow to hidden ensures that`any content with dimensions larger than the container will be hidden from view`.

- The second CSS rule ensures that images scale with the width of the container.

- the height property is set to auto, meaning an image’s height will automatically scale proportionally with the width.

- Finally, the last line will display images as block level elements (rather than inline-block, their default state). This will prevent images from attempting to align with other content on the page (like text), which can add unintended margin to the images.

**It’s worth memorizing the entire example above. It represents a very common design pattern used to scale images and videos proportionally.**

- The example above scales the width of an image (or video) to the width of a container.
- If the image is larger than the container, the vertical portion of the image will overflow and will not display.
- To swap this behavior, you can set max-height to 100% and width to auto (essentially swapping the values).
- This will scale the height of the image with the height of the container instead.
- If the image is larger than the container, the horizontal portion of the image will overflow and not display.

#### Scaling Background Images

**[⬆ back to top](#table-of-contents)**

Background images of HTML elements can also be scaled responsively using CSS properties.

```css
body {
  background-image: url('#');
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
}
```

- the first CSS declaration sets the background image
- The second declaration instructs the CSS compiler to not repeat the image (by default, images will repeat)
- The third declaration centers the image within the element.
- The final declaration is what scales the background image. The image will cover the entire background of the element, all while keeping the image in proportion.
- If the dimensions of the image exceed the dimensions of the container, then only a portion of the image will display.
