# Accessibility

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)
  - [Visual Readability: Scale](#visual-readability-scale)
  - [Visual Readability: Structure](#visual-readability-structure)
  - [Visual Readability: Color](#visual-readability-color)
  - [Contextual Readability: Interactivity](#contextual-readability-interactivity)
  - [Visibility](#visibility)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

- Learn how to build web pages that are accessible to everyone using modern CSS techniques and accessibility standards..

## Notes

_tldr_:

- Text should be at least 18px in size, and paragraphs should have a line-height of at least 1.5 to increase the readability of content.

- Color contrast between elements should be at least 4.5:1 for standard font sizes.

- Interactive elements should have a visual appearance denoting interactivity, such as underlined links and pointer cursor.

- Abbreviated content should be given additional content via the `<abbr>` element.

- When hiding elements from users, use visibility: hidden; or display: none; to hide content from both assisted and non-assisted users.

- Visual display should reflect the structure of the presented elements within the HTML to provide navigational coherence for assisted and non-assisted users.

- Design pages for consumption in different mediums such as print.

### Intro

- Web accessibility is one of those aspects of web development where its presence, when implemented correctly, goes unnoticed, but its absence can destroy the user experience.

- There are a variety of considerations to make when making a website accessible, ranging from the size and color of the text to invisible properties that are processed by assistive tools such as screen readers

- looking to build an amazing, inclusive experience for all users.

### Visual Readability: Scale

**[⬆ back to top](#table-of-contents)**

Font size has a large impact on the readability of content on a web page.

- While the size of a web page’s text largely depends on the typeface used, a minimum size between 18 to 20 pixels is recommended for smaller screen sizes

Line height, or the spacing between lines of text, also contributes heavily to readability

- The default value of line-height in browsers is around 1.2—however, Web Content Accessibility Standards recommend a minimum of 1.5 within paragraphs.

```css
p {
  font-size: 20px;
  line-height: 1.5;
  letter-spacing: 0.05em;
}
```

### Visual Readability: Structure

**[⬆ back to top](#table-of-contents)**

Text alignment and paragraph width on a page are important things to consider when creating the text layout.

We can avoid too much space between words or the paragraph width by using the text-align and width properties to set appropriate values like the below example code.

```css
p {
  text-align: left;
  width: 400px;
}
```

- In the above example, we set the width using px as the unit of measure. A more appropriate unit for this purpose is the`ch` unit, `with 1ch being equivalent to the width of the zero (0) character.` The ch unit also scales with changes in the value of the font-family or font-size property.

```css
p {
  text-align: left;
  min-width: 45ch;
  max-width: 85ch;
}
```

### Visual Readability: Color

**[⬆ back to top](#table-of-contents)**

Oftentimes, text can become lost in the background color, which prevents users, especially those with vision impairments, from reading it.

- The solution to this is to provide adequate contrast between foreground and background elements.

![Color wheel](/Notes/notes/01-HTML-CSS/assets/color-wheel.png)

- Two colors are contrasting when they are from different segments of the color wheel. Thus, the farther apart the segments, the higher the contrast between the two colors.

- The difference between the two colors is known as the contrast ratio and a minimum contrast ratio must be met in order to adhere to accessibility standards.

- This ratio can range from 1:1, where both compared colors are the same, to 21:1, where the two colors are black and white

![Contrast ratio](/Notes/notes/01-HTML-CSS/assets/contrast-ratio.png)

According to the Web Content Accessibility Guidelines (WCAG), contrast ratios are classified using a 3-tier hierarchy:

1. Level A is the minimum level.
2. Level AA includes all Level A and AA requirements. Many organizations strive to meet Level AA.
3. Level AAA includes all Level A, AA, and AAA requirements.

The recommended minimum contrast ratios between the background and its text using these standards are as follows:

- 4.5:1 (AA criteria)—Text that is less than 24px (not bold) and 19px (bold).
- 3:1 (AA criteria)—Text larger than 24px (not bold) and 19px (bold).
- 7:1 (AAA criteria)—Text that is less than 24px (not bold) and 19px (bold).
- 4.5:1 (AAA criteria)—Text larger than 24px (not bold) and 19px (bold).

The [Chrome Canary](https://www.google.com/chrome/canary/) browser has a built-in contrast ratio display inside its developer tools.

Another popular stand-alone tool is [WebAIM](https://webaim.org/resources/contrastchecker/).

- Exo :

Based on the characteristics of the text choose the pair of colors that meet accessibility standards from the following list:

- #FFFFFF (Background) and #FFFF22 (Text)
- #C3C3C3 (Background) and #000000 (Text)
- #FD8480 (Background) and #FD67FF (Text)

Choose the color pair with the minimum contrast ratio of 4.5:1.

### Contextual Readability: Interactivity

**[⬆ back to top](#table-of-contents)**

Another facet of accessibility is providing context clues to users to show them additional information about the elements they are viewing.

As a provider of the information, a web page should make the full definition available to users. This can be done using the `<abbr>` element. Hovering over the text contained in this element will display an alternative name for it to the user.

```html
<abbr title="Cascading Style Sheets">CSS</abbr>
```

Another simple way to convey interactivity is with links. Styling links in a unique way allows users to discern them from other text that may surround them. One way to do this is by underlining links.

```css
a {
  text-decoration: underline;
}
```

Interactive elements such as buttons can also exist.

The most common way to denote interactivity is with the cursor. Using the cursor property, the mouse cursor can be changed to show a visual representation of potential interactivity.

```css
.button {
  cursor: pointer;
}
```

### Contextual Readability: Color

**[⬆ back to top](#table-of-contents)**

Color can also be used as a contextual indicator of intent or meaning.

- To let users know they have visited a link from your page previously, the :visited selector can be used. This selector will apply styling to links that the user has already clicked. The most common style applied via this selector is a color change.

```css
a:visited {
  color: purple;
}
```

- Navigation is another contextual use for color. When elements are focused, they will often have styling applied to denote where the focus is being applied. Without the :focus selector providing additional styling, often using color or shadowing to show focus, users would not know where they were on the page when navigating using means other than a mouse.

```css
input:focus {
  border-color: blue;
}
```

- An additional common use case for contextual use of color is in the indication of success and error states. This can most often be seen in form validation, where accepted field values will turn an input green and rejected fields will be colored red.

![Input with validation](/Notes/notes/01-HTML-CSS/assets/validation.png)

### Visibility

**[⬆ back to top](#table-of-contents)**

Inevitably, at some point, a web page will need to hide content from users somehow. There are a variety of ways to do this, but each one has different use cases and implications.

- The first way that content could be hidden is by hiding it from everyone. In this case, everyone refers to both human users viewing the web page and screen readers. To hide content from everyone in this manner, you can set the `visibility property to hidden` or set the `display property to none`.

- The second method is to hide content only visually. This means that content will still be accessible to screen readers, but not human users.

- The third method is to hide the content semantically, hiding it from screen readers only but displaying it visually. This method is useful for obscuring content that provides no meaningful context to screen reader users, such as decorative icons and repeated text. This can be done by setting the `aria-hidden HTML attribute to 'true':`

```html
<p class="hidden-sentence" aria-hidden="true">
  This is hidden from screen readers!
</p>
```

### Design Reflecting Structure

**[⬆ back to top](#table-of-contents)**

- it is important to order the content on your page to make sense in the absence of styling. This will lead to a uniform experience for all users.

### Accessibility Across Mediums

**[⬆ back to top](#table-of-contents)**

we haven’t been considering the other ways in which users can consume the content of your web page. For example, how does the page look when printed? Luckily, there are some ways a page can be made more accessible, even when viewed on paper.

- One way to do this is to use media queries, specifically the @media print query:

```css
@media print {
  nav {
    display: none;
  }
}
```

- The code snippet causes the nav element to be removed when the page is printed. This is useful for reducing clutter on the page.

- Print media queries can also be used to display values of elements that otherwise couldn’t be understood. For example, we can display the value of the href attribute when printed so the user can still have access to the link. To expose an href in printed form, the following code can be used:

```css
@media print {
  a[href^='http']:after {
    content: ' (' attr(href) ')';
  }
}
```
