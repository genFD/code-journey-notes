# RESPONSIVE DESIGN - MEDIA QUERIES

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)
- [Media queries](#media-queries)
  - [Viewport Meta Tag](#viewport-meta-tag)
  - [Media Queries](#media-queries-1)
  - [Range](#range)
  - [Dots Per Inch (DPI)](#dots-per-inch-dpi)
  - [And Operator](#and-operator)
  - [Breakpoints](#breakpoints)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

- Learn responsive design techniques, like relative sizing units and media queries, to create websites viewable on devices of all sizes.

## Notes

### Media queries

_tldr_:

- When a website responds to the size of the screen it’s viewed on, it’s called a responsive website.

- You can write media queries to help with different screen sizes.

- Adding the viewport `<meta>` tag to our code allows us to control the width and scaling of the viewport so that it’s sized and scaled correctly on all devices.

- Media queries require media features. Media features are the conditions that must be met to render the CSS within a media query.

- Media features can detect many aspects of a user’s browser, including the screen’s width, height, resolution, orientation, and more.

- The and operator requires multiple media features to be true at once.

- A comma separated list of media features only requires one media feature to be true for the code within to be applied.

- The best practice for identifying where media queries should be set is by resizing the browser to determine where the content naturally breaks. Natural breakpoints are found by resizing the browser.

#### Responsive Web Design

- Because screen sizes can vary greatly across different devices, it’s important for websites to resize and reorganize their content to best fit screens of all sizes.

#### Viewport Meta Tag

**[⬆ back to top](#table-of-contents)**

- In order for us to enable this responsive CSS to work, we need to get familiar with the HTML viewport meta tag

- The viewport is the total viewable area for a user.

- It varies depending on device. The viewport is smaller on a mobile device and larger on a desktop

the meta tag `(<meta>)` is used

- to instruct the browser on how to render the webpage’s scale and dimensions.

For instance,

- say if a web page is `960px` and the device is `320px` wide.
- Adding the viewport meta tag `will squish the content down` until it is 320px, removing the need for the user to zoom out and view the whole page!

here's how the syntax is implemented :

```html
!DOCTYPE html>
<html lang="en">
  <head>
    ...
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    ...
  </head>
</html>
```

- the `name="viewport"` attribute: tells the browser to`display the web page at the same width as its screen`

- The content attribute: defines the values for the `<meta>` tag, including width and initial-scale

- The `width=device-width` key-value pair: `controls the size of the viewport` in which it sets the width of the viewport to equal the width of the device.

- the `initial-scale=1` key-value pair: `sets the initial zoom level`

#### Media Queries

**[⬆ back to top](#table-of-contents)**

CSS uses media queries to

- adapt a website’s content to different screen sizes.

With media queries :

- CSS can detect the size of the current screen and apply different CSS styles depending on the width of the screen.

```css
@media only screen and (max-width: 480px) {
  body {
    font-size: 12px;
  }
}
```

- The media query defines a rule for screens smaller than 480 pixels (approximately the width of many smartphones in landscape orientation).

- `@media` — This keyword begins a media query rule and instructs the CSS compiler on how to parse the rest of the rule

- `only screen` — Indicates what types of devices should use this rule. `only` is a `logical operator`. `screen` is a `media type`

- and `(max-width : 480px)` — This part of the rule is called a `media feature`, and instructs the CSS compiler to apply the CSS styles to devices with a width of 480 pixels or smaller. Media features are the conditions that must be met in order to render the CSS within a media query.

- In the example above, the text in the body element is set to a font-size of 12px when the user’s screen is less than 480px.

- ![See media type](https://developer.mozilla.org/en-US/docs/Web/CSS/@media#media_types) for a list of all media types.

- ![See media features ](https://developer.mozilla.org/en-US/docs/Web/CSS/@media#media_features) for a list of all media features.

```html
<div class="box theme-a">Theme A (initial)</div>
<div class="box theme-a adaptive">Theme A (changed if dark preferred)</div>
<br />

<div class="box theme-b">Theme B (initial)</div>
<div class="box theme-b adaptive">Theme B (changed if light preferred)</div>
```

- A common usage is to use a light color scheme by default, and then use prefers-color-scheme: dark to override the colors to a darker variant. It is also possible to do it the other way around.

- This example shows both options: Theme A uses light colors, but can be overridden to dark colors.

- Theme B uses dark colors, but can be overridden to light colors.

- In the end, if the browser supports `prefers-color-scheme`, both themes will be light or dark.

```css
.theme-a {
  background: #dca;
  color: #731;
}
@media (prefers-color-scheme: dark) {
  .theme-a.adaptive {
    background: #753;
    color: #dcb;
    outline: 5px dashed #000;
  }
}
```

- Theme A (brown) uses a light color scheme by default, but will switch to a dark scheme based on the media query

```css
.theme-b {
  background: #447;
  color: #bbd;
}
@media (prefers-color-scheme: light) {
  .theme-b.adaptive {
    background: #bcd;
    color: #334;
    outline: 5px dotted #000;
  }
}
```

- `Theme B (blue)` uses a dark color scheme by default, but will switch to a light scheme based on the media query.

#### Range

**[⬆ back to top](#table-of-contents)**

- By using multiple widths and heights, a range can be set for a media query.

```css
@media only screen and (min-width: 320px) and (max-width: 480px) {
  /* ruleset for 320px - 480px */
}
```

- The example above would apply its CSS rules only when the screen size is between 320 pixels and 480 pixels

- Notice the use of a second and keyword after the min-width media feature. This allows us to chain two requirements together.

The example above can be written using two separate rules as well:

```css
@media only screen and (min-width: 320px) {
  /* ruleset for >= 320px */
}

@media only screen and (min-width: 480px) {
  /* ruleset for >= 480px */
}
```

- The first media query in the example above will apply CSS rules when the size of the screen meets or exceeds 320 pixels

- The second media query will then apply CSS rules when the size of the screen meets or exceeds 480 pixels,

#### Dots Per Inch (DPI)

**[⬆ back to top](#table-of-contents)**

- Many times we will want to supply higher quality media (images, video, etc.) only to users with screens that can support high resolution media.

- Targeting screen resolution also helps users avoid downloading high resolution (large file size) images that their screen may not be able to properly display.

- To target by resolution, we can use the `min-resolution` and `max-resolution` media features.
- These media features accept a resolution value in either `dots per inch (dpi)` or `dots per centimeter (dpc)`

```css
@media only screen and (min-resolution: 300dpi) {
  /* CSS for high resolution screens */
}
```

- The media query in the example above targets high resolution screens by making sure the screen resolution is at least 300 dots per inch

- If the screen resolution query is met, then we can use CSS to display high resolution images and other media.

#### And Operator

**[⬆ back to top](#table-of-contents)**

- The and operator can be used to require multiple media features.
- we can use the and operator to require both a max-width of 480px and to have a min-resolution of 300dpi.

```css
@media only screen and (max-width: 480px) and (min-resolution: 300dpi) {
  /* CSS ruleset */
}
```

- the browser `will require both media features to be true` before it renders the CSS within the media query.

#### Comma Separated List

**[⬆ back to top](#table-of-contents)**

- If only one of multiple media features in a media query must be met, media features can be separated in a comma separated list.

For example, if we needed to apply a style when only one of the below is true:

- The screen is more than 480 pixels wide
- The screen is in landscape mode

```css
@media only screen and (min-width: 480px), (orientation: landscape) {
  /* CSS ruleset */
}
```

- The example above requires only one of the media features to be true for its CSS to apply.

**Note: The orientation media feature detects if the page has more width than height. If a page is wider, it’s considered landscape, and if a page is taller, it’s considered portrait.**

#### Breakpoints

**[⬆ back to top](#table-of-contents)**

- The points at which media queries are set are called breakpoints.
- Breakpoints are the screen sizes at which your web page does not appear properly.
- for example: if we want to target tablets that are in landscape orientation, we can create the following breakpoint

<!-- tablet -->

```css
@media only screen and (min-width: 768px) and (max-width: 1024px) and (orientation: landscape) {
  /* CSS ruleset */
}
```

- The example above creates a screen size range the size of a tablet in landscape mode and also identifies the orientation.

- setting breakpoints for every device imaginable would be incredibly difficult because there are many devices of differing shapes and sizes.

- the best practice is to resize your browser to view where the website naturally breaks based on its content.

- The dimensions at which the layout breaks or looks odd become your media query breakpoints.

- Within those breakpoints, we can adjust the CSS to make the page resize and reorganize.

![Screen sizes](/Notes/notes/01-HTML-CSS/assets/screen-sizes.webp)

- use this list as a reference of screen widths to test your website to make certain it looks great across a variety of devices.
