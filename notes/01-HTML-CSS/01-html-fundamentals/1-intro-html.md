# Intro HTML

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)
  - [What is HTML?](#what-is-html)
  - [Anatomy of an HTML element](#anatomy-of-an-html-element)
  - [HTML document structure](#html-document-structure)
  - [Headings](#headings)
  - [DIVS](#divs)
  - [Attributes](#attributes)
  - [Styling Text](#styling-text)
  - [Line Breaks](#line-breaks)
  - [Unordered Lists](#unordered-lists)
  - [Ordered Lists](#ordered-lists)
  - [Images](#images)
    - [Image Alts](#image-alts)
  - [Videos](#videos)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

- Learning how to add and modify basic content on a page, like text, images, and videos.

## Notes

_TLDR_:

- HTML stands for HyperText Markup Language and is used to create the structure and content of a webpage.

- Most HTML elements contain opening and closing tags with raw text or other HTML tags between them.

- HTML elements can be nested inside other elements. The enclosed element is the child of the enclosing parent element.

- Any visible content should be placed within the opening and closing `<body>` tags.

- Headings and sub-headings, `<h1>` to `<h6>` tags, are used to provide titles for sections of content.

- `<p>`, `<span>` and `<div>` tags specify text or blocks.

- The `<em>` and `<strong>` tags are used to emphasize text.

- Line breaks are created with the `<br>` tag

- Ordered lists `<ol>` are numbered and unordered lists `<ul>` are bulleted

- Images `<img>` and videos `<video>` can be added by linking to an existing source.

### `What is HTML?`

**[⬆ back to top](#table-of-contents)**

- Stands for HyperText Markup Language

- HTML is a markup language that web developers use to `structure and describe the content of a webpage` (not a programming language)

- HTML consists of elements that described different types of content:
  paragraphs, links, headings, images, video, etc.

- Web browsers understand HTML and render HTML code as websites

### `Anatomy of an HTML element`

**[⬆ back to top](#table-of-contents)**

![Anatomy html element](/notes/01-HTML-CSS/assets/anatomy-html.svg)

```html
<p>HTML is a markup language.</p>
```

an element is made of 3 parts :

1. The opening tag : Name of the element wrapped in `<>`
2. The content : In the example above, it's a text but it might be another element a `child element`. Some elements have no contents like `<img/>`
3. The Closing tag : Same as the opening tag but with a forward slash `/`

There are many tags that we can use to organize and display text and other types of content, like image

### `HTML document structure`

**[⬆ back to top](#table-of-contents)**

HTML is organized as a collection of family tree relationships

```html
<body>
  <p>This paragraph is a child of the body</p>
</body>
```

We placed `<p>` tags within `<body>` tags.

- The child element is said to be nested inside of the parent element.

- The relationship between elements and their ancestor and descendent elements is known as hierarchy.
- The reason why understanding the hierarchy is important is `because child elements can inherit behavior and styling from their parent element`.

### `Headings`

**[⬆ back to top](#table-of-contents)**

Headings are used to describe content, like the title of a movie or an educational article.

- There are 6 different headings : h1, h2, h3, h4, h5, h6. Headings helps to create a `visual hierarchy` and break up text into logical parts.

- Each page should have only one `h1`

### `DIVS`

**[⬆ back to top](#table-of-contents)**

`<div>` is short for “division” or a container that divides the page into sections. These sections are very useful for grouping elements in your HTML together.

```html
<body>
  <div>
    <h1>Why use divs?</h1>
    <p>Great for grouping elements!</p>
  </div>
</body>
```

divs are very useful when we want to apply custom styles to our HTML elements.

`<div>`s can contain any text or other HTML elements, such as links, images, or videos.

### `Attributes`

**[⬆ back to top](#table-of-contents)**

Attributes are `content added to the opening tag of an element and can be used in several different ways, from providing information to changing styling`.
Attributes are made up of the following two parts:

- The name of the attribute
- The value of the attribute

One commonly used attribute is the `id`. `ids` have several different purposes in HTML, but for now, we’ll focus on how they can help us identify content on our page.

```html
<div id="intro">
  <h1>Introduction</h1>
</div>
```

#### `Displaying Text`

If you want to display text in HTML, you can use a paragraph or span:

- Paragraphs `<p>` contain a block of plain text.
- `<span>` contains short pieces of text or other HTML. They are used to separate small pieces of content that are on the same line as other content

```html
<div>
  <h1>Technology</h1>
</div>
<div>
  <p>
    <span>Self-driving cars</span> are anticipated to replace up to 2 million
    jobs over the next two decades.
  </p>
</div>
```

In the example above, there are two differents `<div>`. The second `<div>` contains a `<p>` with `<span>Self-driving cars</span>`. This `<span>` element separates “Self-driving cars” from the rest of the text in the paragraph.

When you want to `target a specific piece of content that is inline`, or `on the same line as other text`, It’s best to use a `<span>` element. If you want to divide your content into blocks, it’s better to use a `<div>`.

### `Styling Text`

**[⬆ back to top](#table-of-contents)**

You can also style text using HTML tags. The `<em>` tag emphasizes text, while the `<strong>` tag highlights important text.

- The `<em>` tag will generally render as italic emphasis.
- The `<strong>` will generally render as bold emphasis.

```html
<p>
  <strong>The Nile River</strong> is the <em>longest</em> river in the world,
  measuring over 6,850 kilometers long (approximately 4,260 miles).
</p>
```

“The Nile River” is bolded and “longest” is in italics.

### `Line Breaks`

**[⬆ back to top](#table-of-contents)**

If you are interested in modifying the spacing in the browser, you can use HTML’s line break element: `<br>`. You can use it anywhere within your HTML code and a line break will be shown in the browser.

```html
<p>
  The Nile River is the longest river <br />
  in the world, measuring over 6,850 <br />
  kilometers long (approximately 4,260 <br />
  miles).
</p>
```

The code in the example above will result in an output that looks like the following:

The Nile River is the longest river
in the world, measuring over 6,850
kilometers long (approximately 4,260
miles).

### `Unordered Lists`

**[⬆ back to top](#table-of-contents)**

In HTML, you can use an unordered list tag `(<ul>)` to create a list of items in no particular order. An unordered list outlines individual list items with a bullet point.

**Note:The `<ul>` element should not hold raw text and won’t automatically format raw text into an unordered list of items**

```html
<ul>
  <li>Limes</li>
  <li>Tortillas</li>
  <li>Chicken</li>
</ul>
```

The output will look like this:

- Limes
- Tortillas
- Chicken

### `Ordered Lists`

**[⬆ back to top](#table-of-contents)**

They are useful when you need to list different steps in a process or rank items for first to last.

```html
<ol>
  <li>Preheat the oven to 350 degrees.</li>
  <li>Mix whole wheat flour, baking soda, and salt.</li>
  <li>Cream the butter, sugar in separate bowl.</li>
  <li>Add eggs and vanilla extract to bowl.</li>
</ol>
```

The output will look like this:

1. Preheat the oven to 350 degrees.
2. Mix whole wheat flour, baking soda, and salt.
3. Cream the butter, sugar in separate bowl.
4. Add eggs and vanilla extract to bowl

### `Images`

**[⬆ back to top](#table-of-contents)**

The `<img>` tag allows you to add an image to a web page.

- `<img>` tag is a `self-closing tag`.
- Self-closing tags may include or omit the final slash — both will render properly

```html
<img src="image-location.jpg" />
```

- The `<img>` tag has a required attribute called `src` which must be set to the image’s source, or the location of the image.

#### `Image Alts`

**[⬆ back to top](#table-of-contents)**

- The alt attribute, which means alternative text, brings meaning to the images on our sites.
- The alt attribute can be added to the image tag just like the src attribute. The value of alt should be a description of the image.

```html
<img src="#" alt="A field of yellow sunflowers" />
```

- the alt attribute also serves the following purposes:

  - If an image fails to load on a web page, a user can mouse over the area originally intended for the image and read a brief description of the image.

  - Visually impaired users often browse the web with the aid of screen reading software. When you include the alt attribute, the screen reading software can read the image’s description out loud to the visually impaired user.

  - The alt attribute also plays a role in Search Engine Optimization (SEO), because search engines cannot “see” the images on websites as they crawl the internet. Having descriptive alt attributes can improve the ranking of your site.

### `Videos`

**[⬆ back to top](#table-of-contents)**

- Like the `<img>` element, the `<video>` element requires a src attribute with a link to the video source.

- Unlike the `<img>` element however, the `<video>` element requires an opening and a closing tag.

```html
<video src="myVideo.mp4" width="320" height="240" controls>
  Video not supported
</video>
```

- After the src attribute, the width and height attributes are used to set the size of the video displayed in the browser.

- The controls attribute instructs the browser to include basic video controls such as pausing and playing

- The text, Video not supported, between the opening and closing video tags will only be displayed if the browser is unable to load the video
