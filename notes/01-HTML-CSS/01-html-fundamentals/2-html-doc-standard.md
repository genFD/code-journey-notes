# HTML DOCUMENT STANDARD

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)
  - [Preparing for HTML](#preparing-for-html)
  - [The `<html>` Tag](#the-html-tag)
  - [The `<head>` element](#the-head-element)
  - [Linking to Other Web Pages](#linking-to-other-web-pages)
    - [Opening Links in a New Window](#opening-links-in-a-new-window)
    - [Linking to Relative Page](#linking-to-relative-page)
    - [Linking At Will](#linking-at-will)
    - [Linking to Same Page](#linking-to-same-page)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

- Learning how to set up an HTML file.

## Notes

_TLDR_:

- The `<!DOCTYPE html>` declaration should always be the first line of code in your HTML files. This lets the browser know what version of HTML to expect.

- The `<html>` element will contain all of your HTML code

- Information about the web page, like the title, belongs within the `<head>` of the page.

- You can add a title to your web page by using the `<title>` element, inside of the head.

- A webpage’s title appears in a browser’s tab

- Anchor tags `(<a>)` are used to link to internal pages, external pages or content on the same page.

- You can create sections on a webpage and jump to them using `<a>` tags and adding ids to the elements you wish to jump to.

- Whitespace between HTML elements helps make code easier to read while not changing how elements appear in the browser.

- Indentation also helps make code easier to read. It makes parent-child relationships visible.

- Comments are written in HTML using the following syntax: `<!-- comment -->`.

### `Preparing for HTML`

**[⬆ back to top](#table-of-contents)**

- We can let web browsers know that we are using HTML by starting our document with a document type declaration.

The declaration looks like this:

```html
<!DOCTYPE html>
```

It tells the browser what type of document to expect, along with what version of HTML is being used in the document. The browser will correctly assume that the html in <!DOCTYPE html> is referring to HTML5, as it is the current standard.

### The `<html>` Tag

**[⬆ back to top](#table-of-contents)**

The <!DOCTYPE html> doesn’t actually add any HTML structure or content.

- To create HTML structure and content, we must add opening and closing `<html>` tags after declaring `<!DOCTYPE html>`

```html
<!DOCTYPE html>
<html></html>
```

Anything between the opening `<html>` and closing `</html>` tags will be interpreted as HTML code.

### The `<head>` element

**[⬆ back to top](#table-of-contents)**

So far you’ve done two things to set up the file properly:

- Declared to the browser that your code is HTML with <!DOCTYPE html>
- Added the HTML element `(<html>)` that will contain the rest of your code.

Let’s also give the browser some information about the page itself. We can do this by adding a `<head>` element.

- The `<head>` element contains the metadata for a web page.
- Metadata is information about the page that isn’t displayed directly on the web page, unlike the information inside of the `<body>` tag.
- Examples of metadata include: the `title` of the web page.
  A browser’s tab displays the title specified in the `<title>` tag. The `<title>` tag is always inside of the `<head>`.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>The new title</title>
  </head>
</html>
```

### `Linking to Other Web Pages`

**[⬆ back to top](#table-of-contents)**

You can add links to a web page by adding an anchor element `<a>` and including the text of the link in between the opening and closing tags

```html
<a>This Is A Link To Wikipedia</a>
```

The anchor element in the example above is incomplete without the href attribute. This attribute stands for `hyperlink` reference and is used to link to a path, or the address to where a file is located

```html
<a href="https://www.wikipedia.org/">This Is A Link To Wikipedia</a>
```

#### `Opening Links in a New Window`

**[⬆ back to top](#table-of-contents)**

- The `target` attribute specifies how a link should open.

- For a link to open in a new window(tab), the target attribute requires a value of `_blank`

```html
<a href="https://en.wikipedia.org/wiki/Brown_bear" target="_blank"
  >The Brown Bear</a
>
```

#### `Linking to Relative Page`

**[⬆ back to top](#table-of-contents)**

How to link to internal web pages like Home, About, and Contact?

```md
project-folder/
|—— about.html
|—— contact.html
|—— index.html
```

The example above shows three different files — about.html, contact.html, and index.html in one folder.

Because the files are stored in the same folder, we can link web pages together using a relative path.

```html
<a href="./contact.html">Contact</a>
```

#### `Linking At Will`

**[⬆ back to top](#table-of-contents)**

So far, we’ve added links that were made up of only text, like the following:

```html
<a href="https://en.wikipedia.org/wiki/Opuntia" target="_blank">Prickly Pear</a>
```

HTML allows you to turn nearly any element into a link by wrapping that element with an anchor element. With this technique, it’s possible to turn images into links by simply wrapping the `<img>` element with an `<a>` element.

```html
<a href="https://en.wikipedia.org/wiki/Opuntia" target="_blank"
  ><img
    src="https://www.Prickly_Pear_Closeup.jpg"
    alt="A red prickly pear fruit"
/></a>
```

#### `Linking to Same Page`

**[⬆ back to top](#table-of-contents)**

How do we make it easier for a user to jump to different portions of our page?
When users visit our site, we want them to be able to click a link and have the page automatically scroll to a specific section.

In order to link to a target on the same page, we must give the target an `id`, like this:

```html
<p id="top">This is the top of the page!</p>
<h1 id="bottom">This is the bottom!</h1>
```

The target link is a string containing the `#` character and the target element’s id.

```html
<ol>
  <li><a href="#top">Top</a></li>
  <li><a href="#bottom">Bottom</a></li>
</ol>
```

#### Project

--> BLOG
