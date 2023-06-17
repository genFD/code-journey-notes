# TYPOGRAPHY

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

## Notes

`Multi-Word Values`
When specifying a typeface with multiple words, like Times New Roman, it is recommended to use quotation marks (' ') to group the words together, like so:

```CSS
h1 {
  font-family: 'Times New Roman';
}
```

`Web Safe Fonts`

There is a selection of fonts that will appear the same across all browsers and operating systems. These fonts are referred to as web safe fonts. list here -> [list](https://www.cssfontstack.com/)

`Fallback Fonts and Font Stacks`

Web safe fonts are good fallback fonts that can be used if your preferred font is not available.

```css
h1 {
  font-family: Caslon, Georgia, 'Times New Roman';
}
```

In the example above, Georgia and Times New Roman are fallback fonts to Caslon.
When you specify a group of fonts, you have what is known as a font stack. A `font stack` usually contains a list of `similar-looking fonts`.

Here, the browser will first try to use the Caslon font. If that’s not available, it will try to use a similar font, Georgia. And if Georgia is not available, it will try to use Times New Roman

`Serif and Sans-Serif`

You may be wondering what features make a font similar to another font. The fonts Caslon, Georgia, and Times New Roman are Serif fonts.

- Serif fonts have `extra details on the ends of each letter`, as opposed to Sans-Serif fonts, which do not have the extra details.

![Serif vs Sans serif](/Notes/notes/01-HTML-CSS/assets/htmlcss1-diagram__fontanatomy.svg)

- `serif` and `sans-serif` are also keyword values that can be added as a final fallback font if nothing else in the font stack is available.

```css
h1 {
  font-family: Caslon, Georgia, 'Times New Roman', serif;
}
```

### Font Weight

the font-weight property controls how bold or thin text appears. It can be specified with keywords or numerical values

`Keyword Values`

- The font-weight property can take any one of these keyword values:

`bold`: Bold font weight.
`normal`: Normal font weight. This is the default value.
`lighter`: One font weight lighter than the element’s parent value.
`bolder`: One font weight bolder than the element’s parent value.

`Numerical Values`

Numerical values can range from 1 (lightest) to 1000 (boldest), but it is common practice to use increments of 100.

`A font weight of 400 is equal to the keyword value normal`
`A font weight of 700 is equal to bold`

```css
.left-section {
  font-weight: 700;
}

.right-section {
  font-weight: bold;
}
```

### Font Style

```css
h3 {
  font-style: italic;
}
```

The `italic value` causes text to appear in italics. The font-style property also has a normal value which is the default.

### Text transformation

Text can also be styled to appear in either all uppercase or lowercase with the text-transform property.

```css
h1 {
  text-transform: uppercase;
}
```

### Text layout

#### Letter Spacing

The letter-spacing property is used to `set the horizontal spacing between the individual characters in an element`. It’s not common to set the spacing between letters, but it can sometimes help the readability of certain fonts or styles. The letter-spacing property takes length values in units, such as 2px or 0.5em.
In the example below, each character in the paragraph element will be separated by 2 pixels.

```css
p {
  letter-spacing: 2px;
}
```

#### Word Spacing

You can set the space between words with the `word-spacing property`. It’s also not common to increase the spacing between words, but it may help enhance the readability of bolded or enlarged text. The word-spacing property also takes length values in units, such as 3px or 0.2em.

```css
p {
  letter-spacing: 2px;
}
```

#### Line Height

We can use the `line-height` property `to set how tall we want each line containing our text to be`. Line height values can be a unitless number, such as 1.2, or a length value, such as 12px, 5% or 2em.

![Line height](/Notes/notes/01-HTML-CSS/assets/htmlcss1-diagram__leading_updated_1-01.svg)

```css
p {
  line-height: 1.4;
}
```

In the example above, the height between lines is set to 1.4. Generally, the unitless value is preferred since it is responsive based on the current font size. In other words, if the line-height is specified by a unitless number, changing the font size will automatically readjust the line height.

#### Text Alignment

The text-align property, which you may already be familiar with from the CSS Visual Rules lesson, aligns text to its parent element.

```css
h1 {
  text-align: right;
}
```

### Web fonts

The fonts you can use for your website are limitless—web fonts allow you to express your unique style through a multitude of different fonts found on the web.

Free font services, like [Google Fonts](https://fonts.google.com/) and [Adobe Fonts](https://fonts.adobe.com/) host fonts that you can link to from your HTML document with a provided `<link>` element.

You can also use fonts from paid font distributors like [fonts.com](https://www.fonts.com/) by downloading and hosting them with the rest of your site’s files. You can create a `@font-face` ruleset in your CSS stylesheet to link to the relative path of the font file.

### Web Fonts Using `<link>`

When you’re done selecting a font and its styles, you can review your selected font family, and a `<link>`element will be automatically generated for you to use on your site!

```html
<head>
  <!-- Add the link element for Google Fonts along with other metadata -->
  <link
    href="https://fonts.googleapis.com/css2?family=Roboto:wght@100&display=swap"
    rel="stylesheet"
  />
</head>
```

The generated `<link>`element needs to be added to the `<head>` element in your HTML document for it to be ready to be used in your CSS.

```css
p {
  font-family: 'Roboto', sans-serif;
}
```

### Web Fonts Using @font-face

fonts can be downloaded just like any other file on the web. They come in a few different file formats, such as:

- OTF (OpenType Font)
- TTF (TrueType Font)
- WOFF (Web Open Font Format)
- WOFF2 (Web Open Font Format 2)

It’s a good idea to include TTF, WOFF, and WOFF2 formats with your @font-face rule to ensure compatibility on all browsers.

Within the “Selected Families” section, you can use the “Download” button to download the font files to your computer. The files will be downloaded in a single format.

When you have the files you need, move them to a folder inside your website’s working directory, and you’re ready to use them in a @font-face ruleset!

```css
@font-face {
  font-family: 'MyParagraphFont';
  src: url('fonts/Roboto.woff2') format('woff2'), url('fonts/Roboto.woff')
      format('woff'), url('fonts/Roboto.ttf') format('truetype');
}
```

- The @font-face at-rule is used as the selector. It’s recommended to define the @font-face ruleset at the top of your CSS stylesheet.

- inside the declaration block, the font-family property is used to set a custom name for the downloaded font. The name can be anything you choose, but it must be surrounded by quotation marks. In the example, the font is named `'MyParagraphFont'`, as this font will be used for all paragraphs

- The src property contains three values, each specifying the relative path to the font file and its format. In this example, the font files are stored inside a folder named fonts within the working directory.

- the ordering for the different formats is important because our browser will start from the top of the list and search until it finds a font format that it supports.

- Once the @font-face at-rule is defined, you can use the font in your stylesheet!

```css
p: {
  font-family: 'MyParagraphFont', sans-serif;
}
```
