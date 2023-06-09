# THE DOCUMENT OBJECT MODEL

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)
- [What is the DOM?](#what-is-the-dom)
- [The DOM as a Tree Structure](#the-dom-as-a-tree-structure)
- [Parent Child Relationships in the DOM](#parent-child-relationships-in-the-dom)
- [Nodes and Elements in the DOM](#nodes-and-elements-in-the-dom)
- [Attributes of Element Node](#attributes-of-element-node)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

## Notes

_TLDR_:

- The DOM is a structural model of a web page that allows for scripting languages to access that page.

- The system of organization in the DOM mimics the nesting structure of an HTML document.

- Elements nested within another are referred to as the children of that element. The element they are nested within is called the parent element of those elements.

- The DOM also allows access to the attributes of an HTML element such as style, id, etc.

### What is the DOM?

**[⬆ back to top](#table-of-contents)**

The Document Object Model, abbreviated DOM, is a powerful tree-like structure that `allows programmers to conceptualize hierarchy and access the elements on a web page`.

a useful way to understand what `DOM` does is by breaking down the acronym but out of order:

- The DOM is a logical tree-like Model that `organizes a web page’s HTML Document as an Object`.

The DOM is implemented by browsers to allow `JavaScript to access, modify, and update the structure of an HTML web page in an organized way`.

### The DOM as a Tree Structure

**[⬆ back to top](#table-of-contents)**

Perhaps you’re already familiar with the concept of family trees: these charts represent the familial relationships amongst the descendants of a given family name.
In computer science, we would call each family member a node.

We define a `node` as an `intersecting point` in a tree that contains data.

In the DOM tree, the top-most node is called the `root node`, and it represents the HTML document. The descendants of the root node are the `HTML tags` in the document, starting with the `<html>` tag followed by the `<head>` and `<body>` tags and so on.

### Parent Child Relationships in the DOM

**[⬆ back to top](#table-of-contents)**

Following the metaphor of a family tree, let’s define some key terminology in the DOM hierarchy:

`A parent node` is any node that is `a direct ancestor of another node.`
A child node is a`direct descendant of another node, called the parent node.`

### Nodes and Elements in the DOM

**[⬆ back to top](#table-of-contents)**

```html
<!DOCTYPE html>
<html>
  <head>
    <title>The title</title>
  </head>
  <body>
    <h1>The heading</h1>
    <div>
      <p>A summary</p>
      <p>A description</p>
    </div>
  </body>
</html>
```

![Tree node](/notes/02-language-mastery/assets/domTreeEx4.svg)

A node is an intersecting point in a tree that also contains data.
There are multiple types of node objects in the DOM tree.

- In our diagram, the node objects with the sharp-edge rectangles are Element nodes
- the rounded edge rectangles are Text nodes, because they represent the text inside the HTML paragraph elements.

When trying to modify a web page, the script will mostly interact with the DOM element nodes and occasionally text nodes.
In the diagram to the right, the DOM element nodes are highlighted red. These correspond to elements in the HTML document.

### Attributes of Element Node

**[⬆ back to top](#table-of-contents)**

![Attributes](/notes/02-language-mastery/assets/domTreeattributes.svg)

Much like an element in an HTML page, the DOM allows us to access a node’s attributes, such as its `class`, `id`, and `inline style`.

In the diagram to the right, we have highlighted the paragraph element with an id of 'bio' in the HTML document. If we were accessing that element node in our script, the DOM would allow us to tweak each of those attributes, or simply access them to check their value in the code
