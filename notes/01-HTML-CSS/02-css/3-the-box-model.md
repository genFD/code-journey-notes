# BOX MODEL

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

All elements on a web page are interpreted by the browser as “living” inside of a box. This is what is meant by the box model

The main objective is to :

- Learn how to use the Box Model to position HTML elements on a web page.

## Notes

_tldr_:

- The box model comprises a set of properties used to create space around and between HTML elements.

- The height and width of a content area can be set in pixels or percentages.

- Borders surround the content area and padding of an element. The color, style, and thickness of a border can be set with CSS properties.

- Padding is the space between the content area and the border. It can be set in pixels or percent.

- Margin is the amount of spacing outside of an element’s border.

- Horizontal margins add, so the total space between the borders of adjacent elements is equal to the sum of the right margin of one element and the left margin of the adjacent element.

- Vertical margins collapse, so the space between vertically adjacent elements is equal to the larger margin.

- margin: 0 auto horizontally centers an element inside of its parent content area, if it has a width.

- The overflow property can be set to display, hidden, or scroll, and dictates how HTML will render content that overflows its parent’s content area.

- The visibility property can hide or show elements.

### The Box Model

**[⬆ back to top](#table-of-contents)**

![Box model](/Notes/notes/01-HTML-CSS/assets/diagram-boxmodel.svg)

The model includes the content area’s size (width and height) and the element’s padding, border, and margin.

1. `width and height`: The width and height of the content area.
2. `padding`: The amount of space between the content area and the border.
3. `border`: The thickness and style of the border surrounding the content area and padding.
4. `margin`: The amount of space between the border and the outside edge of the element.

### Height and Width

**[⬆ back to top](#table-of-contents)**

An element’s content has two dimensions: a height and a width
The CSS height and width properties can be used to modify these default dimensions

```css
p {
  height: 80px;
  width: 240px;
}
```

In this example, the height and width of paragraph elements are set to 80 pixels and 240 pixels. Pixels allow you to set the exact size of an element’s box (width and height).

When you set the width and height of an element in pixels :
`it will be the same size on all devices — an element that fills a laptop screen will overflow a mobile screen`.

### Borders

**[⬆ back to top](#table-of-contents)**

A border is a line that surrounds an element, like a frame around a painting. Borders can be set with a specific width, style, and color:

`width` — The thickness of the border. A border’s thickness can be set in pixels or with one of the following keywords: thin, medium, or thick.

`style` — The design of the border. Web browsers can render any of 10 different styles. Some of these styles include: none, dotted, and solid.

`color` — The color of the border. Web browsers can render colors using a few different formats, including

```css
p {
  border: 3px solid coral;
}
```

The default border is medium none color, where color is the current color of the element.

### Border radius

**[⬆ back to top](#table-of-contents)**

a border doesn’t have to be square.

You can modify the corners of an element’s border box with the border-radius property.

```css
div.container {
  border: 3px solid blue;
  border-radius: 5px;
}
```

The code in the example above will set all four corners of the border to a radius of 5 pixels (i.e. the same curvature that a circle with a radius of 5 pixels would have).

You can create a border that is a perfect circle by first creating an element with the same width and height, and then setting the radius equal to half the width of the box, which is 50%.

```css
div.container {
  height: 60px;
  width: 60px;
  border: 3px solid blue;
  border-radius: 50%;
}
```

### Padding

**[⬆ back to top](#table-of-contents)**

The space between the contents of a box and the borders of a box is known as padding.
In CSS, you can modify this space with the padding property.

```css
p.content-header {
  border: 3px solid coral;
  padding: 10px;
}
```

The code in this example puts 10 pixels of space between the content of the paragraph (the text) and the borders, on all four sides

The padding property is often used to expand the background color and make the content look less cramped.
If you want to be more specific about the amount of padding on each side of a box’s content, you can use the following properties:

- padding-top
- padding-right
- padding-bottom
- padding-left

Each property affects the padding on only one side of the box’s content, giving you more flexibility in customization.

```css
p.content-header {
  border: 3px solid fuchsia;
  padding-bottom: 10px;
}
```

### Margin

**[⬆ back to top](#table-of-contents)**

The fourth and final component of the box model is `margin`.

Margin refers to the space directly outside of the box. The `margin` property is used to specify the size of this space.

```css
p {
  border: 1px solid aquamarine;
  margin: 20px;
}
```

The code in the example above will place 20 pixels of space on the outside of the paragraph’s box on all four sides.

If you want to be even more specific about the amount of margin on each side of a box, you can use the following properties:

- margin-top
- margin-right
- margin-bottom
- margin-left

The margin property also lets you center content. However, you must follow a few syntax requirements. Take a look at the following example:

```css
div.headline {
  width: 400px;
  margin: 0 auto;
}
```

In the example above, margin: 0 auto; will `center the divs in their containing elements`.
The 0 sets the top and bottom margins to 0 pixels. The auto value instructs the browser to adjust the left and right margins until the element is centered within its containing element.

**In order to center an element, a width must be set for that element.**.
Otherwise, the width of the div will be automatically set to the full width of its containing element, like the `<body>`, for example. It’s not possible to center an element that takes up the full width of the page, since the width of the page can change due to display and/or browser window size.

In the example above, the width of the div is set to 400 pixels, which is less than the width of most screens. **This will cause the div to center within a containing element that is greater than 400 pixels wide.**

### Margin Collapse

**[⬆ back to top](#table-of-contents)**

As you have seen, padding is space added inside an element’s border, while margin is space added outside an element’s border. One additional difference is that `top and bottom margins, also called vertical margins, collapse,` while top and bottom padding does not.

Horizontal margins (left and right), like padding, are always displayed and added together.

if two divs with ids #div-one and #div-two, are next to each other, they will be as far apart as the sum of their adjacent margins.

```css
#img-one {
  margin-right: 20px;
}

#img-two {
  margin-left: 20px;
}
```

In this example, the space between the `#img-one` and `#img-two` borders is 40 pixels.

Unlike horizontal margins, vertical margins do not add. Instead, the larger of the two vertical margins sets the distance between adjacent elements.

```css
#img-one {
  margin-top: 30px;
}

#img-two {
  margin-bottom: 20px;
}
```

In this example, the vertical margin between the `#img-one` and `#img-two` elements is 30 pixels.
Although the sum of the margins is 50 pixels, the margin `collapses` so the spacing is only dependent on the `#img-one` bottom margin.

![Margin Collapse](/Notes/notes/01-HTML-CSS/assets/margin_collapse.svg)

- Elements A and B have 20 pixels of horizontal margin, the sum of each element’s margin. - Elements A and C have 30 pixels of vertical margin — the top margin of element C.

### Minimum and Maximum Height and Width

**[⬆ back to top](#table-of-contents)**

Because a web page can be viewed through displays of differing screen size,
the content on the web page can suffer from those changes in size. To avoid this problem, CSS offers two properties that can limit how narrow or how wide an element’s box can be sized to:

`min-width` — this property ensures a minimum width of an element’s box.
`max-width` — this property ensures a maximum width of an element’s box.

```css
p {
  min-width: 300px;
  max-width: 600px;
}
```

In the example above, the width of all paragraphs `will not shrink below 300 pixels`, nor will the `width exceed 600 pixels`.

You can also limit the minimum and maximum height of an element:

`min-height` — this property ensures a minimum height for an element’s box.
`max-height` — this property ensures a maximum height of an element’s box.

```css
p {
  min-height: 150px;
  max-height: 300px;
}
```

In the example above, the height of all paragraphs `will not shrink below 150 pixels` and the `height will not exceed 300 pixels`.

### Overflow

**[⬆ back to top](#table-of-contents)**

All of the components of the box model comprise an element’s size. For example, an image that has the following dimensions is `364 pixels wide and 244 pixels tall`.

- 300 pixels wide
- 200 pixels tall
- 10 pixels padding on the left and right
- 10 pixels padding on the top and bottom
- 2 pixels border on the left and right
- 2 pixels border on the top and bottom
- 20 pixels margin on the left and right
- 10 pixels margin on the top and bottom

The total dimensions (364px by 244px) are calculated by adding all of the vertical dimensions together and all of the horizontal dimensions together. Sometimes, these components result in an element that is larger than the parent’s containing area

**How can we ensure that we can view all of an element that is larger than its parent’s containing area ?**

The overflow property controls what happens to content that spills, or overflows, outside its box. The most commonly used values are:

`hidden` — when set to this value, any content that overflows will be hidden from view.
`scroll` — when set to this value, a scrollbar will be added to the element’s box so that the rest of the content can be viewed by scrolling.
`visible` — when set to this value, the overflow content will be displayed outside of the containing element. Note, this is the default value.

```css
p {
  overflow: scroll;
}
```

In the example above, if any of the paragraph content overflows (perhaps a user resizes their browser window), a scrollbar will appear so that users can view the rest of the content
The overflow property is set on a parent element to instruct a web browser on how to render child elements. For example, if a div’s overflow property is set to scroll, all children of this div will display overflowing content with a scroll bar.

### Resetting Defaults

**[⬆ back to top](#table-of-contents)**

All major web browsers have a default stylesheet they use in the absence of an external stylesheet. These default stylesheets are known as user agent stylesheets. In this case, the term user agent is a technical term for the browser.

User agent stylesheets often have default CSS rules that set default values for padding and margin. This affects how the browser displays HTML elements, which can make it difficult for a developer to design or style a web page.

Many developers choose to reset these default values so that they can truly work with a clean slate.

```css
* {
  margin: 0;
  padding: 0;
}
```

The code in the example above resets the default margin and padding values of all HTML elements. It is often the first CSS rule in an external stylesheet.

### Visibility

**[⬆ back to top](#table-of-contents)**

Elements can be hidden from view with the `visibility property`.

The visibility property can be set to one of the following values:

`hidden` — hides an element.
`visible` — displays an element.
`collapse` — collapses an element.

```html
<ul>
  <li>Explore</li>
  <li>Connect</li>
  <li class="future">Donate</li>
</ul>
```

```css
.future {
  visibility: hidden;
}
```

In the example above, the list item with a class of future will be hidden from view in the browser

**Note**: What’s the difference between display: none and visibility: hidden?

- An element with display: none will be completely removed from the web page.
- An element with visibility: hidden, however, will not be visible on the web page, but the space reserved for it will.

### Changing the Box Model

**[⬆ back to top](#table-of-contents)**

_tldr:_

- In the default box model, box dimensions are affected by border thickness and padding.
- The box-sizing property controls the box model used by the browser.
- The default value of the box-sizing property is content-box.
- The value for the new box model is border-box.
- The border-box model is not affected by border thickness or padding.

The box model, has an awkward limitation regarding box dimensions. This limitation is best illustrated with an example.

```html
<h1>Hello World</h1>
```

```css
h1 {
  border: 1px solid black;
  height: 200px;
  width: 300px;
  padding: 10px;
}
```

In the example above, a heading element’s box has solid, black, 1 pixel thick borders. The height of the box is 200 pixels, while the width of the box is 300 pixels. A padding of 10 pixels has also been set on all four sides of the box’s content.

Unfortunately, under the current box model, the border thickness and the padding will affect the dimensions of the box.

- The 10 pixels of padding increases the height of the box to 220 pixels and the width to 320 pixels
- the 1-pixel thick border increases the height to 222 pixels and the width to 322 pixels.

Under this box model, **the border thickness and padding are added to the overall dimensions of the box**. This makes it difficult to accurately size a box.

#### Box Model: Content-Box

**[⬆ back to top](#table-of-contents)**

In CSS, the box-sizing property controls `the type of box model` the browser should use when interpreting a web page.

The default value of this property is `content-box`. This is the same box model that is affected by border thickness and padding.

#### Box Model: Border-Box

**[⬆ back to top](#table-of-contents)**

It's possible to reset the entire box model and specify a new one: `border-box`:

```css
* {
  box-sizing: border-box;
}
```

The code in the example above resets the box model to border-box for all HTML elements
This new box model avoids the dimensional issues that exist with content-box.

![border-box](/Notes/notes/01-HTML-CSS/assets/htmlcss1-diagram__borderbox_2.svg)

in this box model, the height and width of the box will remain fixed. The border thickness and padding will be included inside of the box, which means the overall dimensions of the box do not change.

```html
<h1>Hello World</h1>
```

```css
* {
  box-sizing: border-box;
}

h1 {
  border: 1px solid black;
  height: 200px;
  width: 300px;
  padding: 10px;
}
```

In the example above, the height of the box would remain at 200 pixels and the width would remain at 300 pixels. The border thickness and padding would remain entirely inside of the box.

### The Box Model in DevTools

- -> View and edit an HTML element’s box using Chrome DevTools.

All HTML elements are boxes made up of four components:

1. a content container,
2. padding,
3. border,
4. and margin.

we will introduce how Google Chrome’s DevTools can be used to view the box around each element on a web page.

#### View Box Model Dimensions with DevTools

You can use Google Chrome’s DevTools to view the box around every element on a web page.
When you have the DevTools open, navigate to the Elements tab.

![element's tab](/Notes/notes/01-HTML-CSS/assets/element-dev-tool.webp)

In this tab you can view all of the elements on the current page. From this view, you can select the element of interest, which will open a new column on the right side of DevTools. Select the tab labeled Computed on the top of the rightmost column.

![devtools tab](/Notes/notes/01-HTML-CSS/assets/DevToolsTabs.webp)

The selected element’s box should appear at the top of the pane. Hovering over each property of the box will cause the property to be highlighted in the web page.

If you know the element you want to inspect, going through all of the steps listed above is unnecessary. Instead, you can right click the element you want to observe and select the Inspect button. This will display DevTools on the right side of the browser with the element selected in the Elements tab. To view the element’s box, you can select the Computed tab.

#### Modify Box Dimensions

Now that you know how to view the box of an element we’ll modify the box’s values with DevTools.

- double click the property value, assign it a new number, and press enter. You can also adjust the value incrementally by double clicking the value and using the up or down arrow keys.
