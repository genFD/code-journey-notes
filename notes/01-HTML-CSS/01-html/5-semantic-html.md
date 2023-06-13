# SEMANTIC HTML

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)
  - [Introduction to Semantic HTML](#introduction-to-semantic-html)
  - [Header and Nav](#header-and-nav)
  - [Main and Footer](#main-and-footer)
  - [Article and Section](#article-and-section)
  - [The Aside Element](#the-aside-element)
  - [Figure and Figcaption](#figure-and-figcaption)
  - [Audio and Attributes](#audio-and-attributes)
  - [Video and Embed](#video-and-embed)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

- Learning the structure and syntax of a `<form>` and the many elements that populate it.

## Notes

_TLDR_:

- Semantic HTML introduces meaning to a page through specific elements that provide context as to what is in between the tags.

- Semantic HTML is a modern standard and makes a website accessible for people who use screen readers to translate the webpage and improves your website’s SEO.

- `<header>`, `<nav>` , `<main>` and `<footer>` create the basic structure of the webpage.

- `<section>` defines elements in a document, such as chapters, headings, or any other area of the document with the same theme.

- `<article>` holds content that makes sense on its own such as articles, blogs, comments, etc.

- `<aside>` contains information that is related to the main content, but not required in order to understand the dominant information.

- `<figure>` encapsulates all types of media.

- `<figcaption>` is used to describe the media in `<figure>`.

- `<video>`, `<embed>`, and `<audio>` elements are used for media files.

### Introduction to Semantic HTML

**[⬆ back to top](#table-of-contents)**

![Semantic vs Non Semantic](/notes/01-HTML-CSS/assets/Semantic.svg)

- The word semantic means “relating to meaning,” so semantic elements provide information about the content between the opening and closing tags.

- Selecting HTML elements based on their meaning, not on how they are presented.

- Elements such as `<div>` and `<span>` are not semantic elements since they provide no context as to what is inside of those tags.

- By using a `<header>` tag instead of a `<div>`, we provide context as to what information is inside of the opening and closing tag

**Why use Semantic HTML?**

- `Accessibility`: Semantic HTML makes webpages accessible for mobile devices and for people with disabilities as well. This is because screen readers and browsers are able to interpret the code better.

- `SEO`: It improves the website SEO, or Search Engine Optimization, which is the process of increasing the number of people that visit your webpage. With better SEO, search engines are better able to identify the content of your website and weight the most important content appropriately.

- `Easy to Understand`: Semantic HTML also makes the website’s source code easier to read for other web developers.

- To better understand this, you can think of comparing non-semantic HTML to going into a store with no signs on the aisles. Since the aisles aren’t labeled, you don’t know what products are in those aisles

- However, stores that do have signs for each aisle make it a lot easier to find the items you need, just like Semantic HTML.

### Header and Nav

**[⬆ back to top](#table-of-contents)**

- A `<header>` is a container usually for either `navigational links` or `introductory content` containing `<h1>` to `<h6>` headings.

```html
<header>
  <h1>Everything you need to know about pizza!</h1>
</header>
```

- This can be compared to the code below which uses a `<div>` tag instead of a `<header>` tag:

```html
<div id="header">
  <h1>Everything you need to know about pizza!</h1>
</div>
```

- A `<nav>` is used to define a `block of navigation links` such as menus and tables of contents. `<nav>` can be used inside of the `<header>` element but can also be used on its own.

```html
<header>
  <nav>
    <ul>
      <li><a href="#home">Home</a></li>
      <li><a href="#about">About</a></li>
    </ul>
  </nav>
</header>
```

- By using `<nav>` as a way to label our navigation links, it will be easier for not only us, but also for web browsers and screen readers to read the code.

### Main and Footer

**[⬆ back to top](#table-of-contents)**

- The element `<main>` is used to encapsulate `the dominant content within a webpage`.
- This tag is separate from the `<footer>` and the `<nav>` of a web page since these elements don’t contain the principal content.
- By using `<main>` as opposed to a `<div>` element, screen readers and web browsers are better able to identify that whatever is inside of the tag is the bulk of the content.

```html
<main>
  <header>
    <h1>Types of Sports</h1>
  </header>
  <article>
    <h3>Baseball</h3>
    <p>
      The first game of baseball was played in Cooperstown, New York in the
      summer of 1839.
    </p>
  </article>
</main>
```

- `<main>` contains an `<article>` and `<header>` tag with child elements that hold the most important information related to the page.

- The content at the bottom of the subject information is known as the footer, indicated by the `<footer>` element. The footer contains information such as:

1. Contact information
2. Copyright information
3. Terms of use
4. Site Map
5. Reference to top of page links

```html
<footer>
  <p>Email me at Codey@Codecademy.com</p>
</footer>
```

### Article and Section

**[⬆ back to top](#table-of-contents)**

- `<section>` defines elements in a document, such as chapters, headings, or any other area of the document with the same theme.

- For example, content with the same theme such as articles about cricket can go under a single `<section>`.

- A website’s home page could be split into sections for the introduction, news items, and contact information.

```html
<section>
  <h2>Fun Facts About Cricket</h2>
</section>
```

- The `<article>` element holds content that makes sense on its own. `<article>` can hold content such as articles, blogs, comments, magazines, etc.

- An `<article>` tag would help someone using a screen reader understand where the article content (that might contain a combination of text, images, audio, etc.) begins and ends.

```html
<section>
  <h2>Fun Facts About Cricket</h2>
  <article>
    <p>A single match of cricket can last up to 5 days.</p>
  </article>
</section>
```

- In the code above, the `<article>` element containing a fact about cricket was placed inside of the `<section>` element. It is important to note that a `<section>` element could also be placed in an `<article>` element depending on the context.

### The Aside Element

**[⬆ back to top](#table-of-contents)**

- The `<aside>` element is used to mark additional information that can enhance another element but isn’t required in order to understand the main content.

- This element can be used alongside other elements such as `<article>`or `<section>.` Some common uses of the `<aside>` element are for:

- Bibliographies
- Endnotes
- Comments
- Pull quotes
- Editorial sidebars
- Additional information

```html
<article>
  <p>
    The first World Series was played between Pittsburgh and Boston in 1903 and
    was a nine-game series.
  </p>
</article>
<aside>
  <p>Babe Ruth once stated, “Heroes get remembered, but legends never die.”</p>
</aside>
```

- The information within the `<article>` is the important content. Meanwhile the information within the `<aside>` enhances the information in `<article>` but is not required in order to understand it.

### Figure and Figcaption

**[⬆ back to top](#table-of-contents)**

- `<figure>` is an element used to encapsulate media such as an image, illustration, diagram, code snippet, etc, which is referenced in the main flow of the document.

```html
<figure>
  <img src="overwatch.jpg" />
</figure>
```

- In this code, we created a `<figure>` element so that we can encapsulate our `<img>` tag.

- It’s possible to add a caption to the image by using `<figcaption>`.

- `<figcaption>` is an element used to describe the media in the `<figure>`tag.

- Usually, `<figcaption>` will go inside `<figure>`

```html
<figure>
  <img src="overwatch.jpg" />
  <figcaption>This picture shows characters from Overwatch.</figcaption>
</figure>
```

- In the example above, we added a `<figcaption>` into the `<figure>` element to describe the image from the previous example. This helps group the `<figure>` content with the `<figcaption>` content.

### Audio and Attributes

**[⬆ back to top](#table-of-contents)**

- The `<audio>` element is used to embed audio content into a document. Like `<video>`, `<audio>` uses src to link the audio source.

```html
<audio>
  <source src="iAmAnAudioFile.mp3" type="audio/mp3" />
</audio>
```

- We linked our audio file into the browser but now we need to give it controls. This is where attributes come in. Attributes provide additional information about an element.

- There are many attributes for `<audio>` but we’re going to be focusing on `controls` and `src`

1. `controls`: automatically displays the audio controls into the browser such as play and mute.

2. `src`: specifies the URL of the audio file.

For example, here’s how we could add both autoplay functionality and audio controls:

```html
<audio autoplay controls></audio>
```

### Video and Embed

**[⬆ back to top](#table-of-contents)**

- The `<video>` element makes it clear that a developer is attempting to display a video to the user.

- Some attributes that can alter a video playback include:

1. `controls`: When added in, a play/pause button will be added onto the video along with volume control and a fullscreen option.

2. `autoplay`: The attribute which results in a video automatically playing as soon as the page is loaded.

3. `loop`: This attribute results in the video continuously playing on repeat.

```html
<video src="coding.mp4" controls>Video not supported</video>
```

- In the code above, a video file named coding.mp4 is being played. The “Video not supported” will only show up if the browser is unable to display the video.

- `<embed>` tag can embed any media content including videos, audio files, and gifs from an external source.

- The `<embed>` tag is a self-closing tag, unlike the `<video>` element.

- `<embed>` is a deprecated tag and other alternatives, such as `<video>`, `<audio>` and `<img>`, should be used in its place, but is being taught for legacy purposes.

```html
<embed src="download.gif" />
```
