# The script Element

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)
- [JavaScript and HTML](#javascript-and-html)
- [The script tag](#the-script-tag)
- [The src attribute](#the-src-attribute)
- [How are scripts loaded?](#how-are-scripts-loaded)
- [Defer attribute](#defer-attribute)
- [Async attribute](#async-attribute)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

## Notes

_TLDR_:

- HTML creates the skeleton of a webpage, but JavaScript introduces interactivity
- The `<script>` element has an opening and closing tag. You can embed JavaScript code inbetween the opening and closing `<script>` tags.
- You link to external JavaScript files with the src attribute in the opening `<script>` tag
- By default, scripts are loaded and executed as soon as the HTML parser encounters them in the HTML file, the HTML parser waits to load the entire script before from proceeding to parse the rest of the page elements.
- The defer attribute ensures that the entire HTML file has been parsed before the script is executed
- The async attribute will allow the HTML parser to continue parsing as the script is being downloaded, but will execute immediately after it has been downloaded.

### JavaScript and HTML

**[⬆ back to top](#table-of-contents)**

Web programmers use JavaScript to make web pages dynamic and interactive. This powerful scripting language is encapsulated in its own HTML element: the `<script>` element. You can think of this `<script>` element as the door to JavaScript for HTML.

### The `<script>` tag

**[⬆ back to top](#table-of-contents)**

It allows you to add JavaScript code inside an HTML file.

```html
<h1>This is an embedded JS example</h1>
<script>
  function Hello() {
    alert('Hello World')
  }
</script>
```

### The `src` attribute

**[⬆ back to top](#table-of-contents)**

Linking code is preferable because of a programming concept called Separation of Concerns (SoC)

Instead of writing JavaScript in our HTML file, we are going to write it in its own file, and then reference this code with a file path name.

We will do this using : the `src` attribute

```html
<script src="./exampleScript.js"></script>
```

### How are scripts loaded?

**[⬆ back to top](#table-of-contents)**

let’s take a moment to talk about how browsers parse HTML files into web pages. This informs where to include a `<script>` element inside your HTML file.

- HTML parsers help browsers render the elements accordingly.
- Elements, including the `<script>` element, are by default, parsed in the order they appear in the HTML file.
- When the HTML parser encounters a `<script>` element, it loads the script then executes its contents before parsing the rest of the HTML.

The two main points to note :

- The HTML parser does NOT process the next element in the HTML file until it loads and executes the `<script>` element, thus leading to a delay in load time and resulting in a poor user experience.

- Additionally, scripts are loaded sequentially, so if one script depends on another script, they should be placed in that very order inside the HTML file.

![Script](/notes/02-language-mastery/assets/ScriptNoAttribute.gif)

The GIF below displays two scripts being loaded :

- The first script makes a Watering Can appear
- The second script makes a Flower appear.

This shows how scripts are loaded sequentially, and how they pause the HTML parser, which is why “Blooming” appears at the end.

### Defer attribute

**[⬆ back to top](#table-of-contents)**

- When the HTML parser comes across a `<script>` element, it stops to load its content
- Once loaded, the JavaScript code is executed and the HTML parser proceeds to parse the next element in the file. This can result in a slow load time for your website

- HTML4 introduced the defer and async attributes of the `<script>` element to address the user wait-time in the website based on different scenarios.

- The defer attribute specifies scripts should be executed after the HTML file is completely parsed

- When the HTML parser encounters a `<script>` element with the defer attribute, it loads the script but defers the actual execution of the JavaScript until after it finishes parsing the rest of the elements in the HTML file

```html
<script src="example.js" defer></script>
```

`When is defer useful?`

When a script contains functionality that requires interaction with the DOM, the defer attribute is the way to go. This way, it ensures that the entire HTML file has been parsed before the script is executed.

### Async attribute

**[⬆ back to top](#table-of-contents)**

- The `async` attribute loads and executes the script asynchronously with the rest of the webpage.

This means that, similar to the defer attribute, the HTML parser will continue parsing the rest of the HTML as the script is downloaded in the background.
However, with the `async` attribute, the script will not wait until the entire page is parsed:

- it will execute immediately after it has been downloaded. Here is an example of the async tag.

```html
<script src="example.js" async></script>
```

`When is it useful?`

`async` is useful for scripts that are independent of other scripts in order to function accordingly. Thus, if it does not matter exactly at which point the script file is executed, asynchronous loading is the most suitable option as it optimizes web page load time.
