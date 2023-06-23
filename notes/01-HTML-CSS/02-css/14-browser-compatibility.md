# BROWSER COMPATIBILITY

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)

- [📝 Notes](#notes)
  - [Polyfills](#polyfills)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

- Learn about browser compatibility and the different techniques to create a consistent user experience regardless of the browser.

## Notes

_tldr_:

- We can check whether a CSS feature is available on specific browsers using tools like caniuse.com.

- Browsers have default styles which we can check using resources like browserdefaultstyles.com.

- We sometimes need to use browser-specific prefixes, especially for new CSS features.

- Polyfills provide a way to support newer web features on older browsers.

### Intro

Every now and then, exciting new HTML, CSS, and JavaScript features get released. However, these new features may not be immediately usable because different browsers adopt new web features at different speeds. For example, Internet Explorer did not support many CSS3 features until Microsoft Edge was released, even though many other browsers had already adopted these features by that point.

The availability of CSS features differs across browsers and their versions. Using a CSS property supported only on select browsers will create inconsistent experiences for users. Thankfully, there are tools such as [caniuse.com](https://caniuse.com/) that can help you find out what browsers support a particular CSS feature

### Browser Defaults

**[⬆ back to top](#table-of-contents)**

Browsers are based on browser engines—the core component responsible for rendering HTML, CSS, and JavaScript in the browser. For example, Chrome, Opera, and Edge use Blink as their browser engine, whereas Firefox uses Gecko and Safari uses Webkit. Each of these engines renders features such as margins, padding, colors, or text slightly differently. This is because different browsers have different _default styles_.

For example, the default text color of the `<a>` element in most browsers is #0000EE, but in Internet Explorer 11, the value is #0066cc. We can use tools such as browserdefaultstyles.com to compare default styles across browsers.

There are also tools that we can use, such as [normalize.css](https://necolas.github.io/normalize.css/) and [minireset.css](https://github.com/jgthms/minireset.css), to eliminate differences between browser default styles and ensure that all elements are rendered the same way across browsers.

```css
ul {
  list-style-image: url('bean.png');
}
```

### Vendor Prefixes

**[⬆ back to top](#table-of-contents)**

- prefixes for property names that were developed as a way of testing new CSS features or standards before browsers fully supported them

Common vendor prefixes include:

- -webkit- for Chrome, Safari, newer versions of Opera, and almost all iOS browsers, including Firefox for iOS
- -moz- for Firefox
- -ms- for Internet Explorer and Microsoft Edge
- -o- for old pre-WebKit versions of Opera

Eventually, these experimental features are fully adopted into all browsers, at which point the vendor prefix is no longer required. Remember that browsers adopt features at different rates—for newer CSS features, you may need to include prefixed property names alongside. An example of this for the transform property would be as follows:

```css
.tilted {
  -webkit-transform: rotate(15deg);
  -moz-transform: rotate(15deg);
  -ms-transform: rotate(15deg);
  -o-transform: rotate(15deg);
  transform: rotate(15deg);
}
```

Rather than looking through every CSS feature on your site to determine which features require vendor prefixes, you can also use tools such as [Autoprefixer](https://github.com/postcss/autoprefixer) to automatically include all vendor prefixes for the features that require them.

### Polyfills

**[⬆ back to top](#table-of-contents)**

- JavaScript codes that allow older browsers to behave as though they understand more advanced features than they actually do.

- These codes rewrite the HTML and CSS codes to simulate features that have not yet been adopted by that version of the browser.

For example, the text-shadow property is not supported in some older versions of Internet Explorer and Firefox but can be simulated using other CSS properties. Using JavaScript, we can replace CSS declarations depending on which browser version a user is using to view the website.

- For a more comprehensive solution, we can use tools such as [Modernizr](https://github.com/Modernizr/Modernizr) or [Polyfill.io](https://github.com/financial-times/polyfill-service) to automatically identify and provide all of the polyfills that our website might need, ensuring that it can run as smoothly as possible on older browsers.

```html
<script src="https://polyfill.io/v3/polyfill.min.js"></script>
```
