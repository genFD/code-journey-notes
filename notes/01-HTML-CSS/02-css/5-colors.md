# COLOR

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

_tldr_:

There are four ways to represent color in CSS:

`Named colors` — there are more than 140 named colors

`Hexadecimal or hex colors`

- Hexadecimal is a number system that has sixteen digits, 0 to 9 followed by “A” to “F”.
- Hex values always begin with # and specify values of red, blue, and green using hexadecimal numbers such as #23F41A.
- Six-digit hex values with duplicate values for each RGB value can be shorted to three digits.

`RGB`

- RGB colors use the rgb() syntax with one value for red, one value for blue and one value for green.

- RGB values range from 0 to 255 and look like this: rgb(7, 210, 50).

`HSL`

- HSL stands for hue (the color itself), saturation (the intensity of the color), and lightness (how light or dark a color is).

- Hue ranges from 0 to 360 and saturation and lightness are both represented as percentages like this: hsl(200, 20%, 50%).

- You can add opacity to color in RGB and HSL by adding a fourth value, a, which is represented as a percentage.

### INTRO

Colors in CSS can be described in three different ways:

`Named colors` — English words that describe colors, also called keyword colors
`RGB` — numeric values that describe a mix of red, green, and blue
`HSL` — numeric values that describe a mix of hue, saturation, and lightness

### Hexadecimal

A `hex color` begins with a hash character (#) which is followed by three or six characters. The characters represent values for red, blue and green.

The hexadecimal number system has 16 digits (0-15) instead of 10 (0-9) like in the standard decimal system.

To represent 10-15, we use A-F.

```md
darkseagreen: #8FBC8F
sienna: #A0522D
saddlebrown: #8B4513
brown: #A52A2A
black: #000000 or #000
white: #FFFFFF or #FFF
aqua: #00FFFF or #0FF
```

### RGB Colors

There is another syntax for representing RGB values that uses decimal numbers rather than hexadecimal numbers, and it looks like this:

```css
h1 {
  color: rgb(23, 45, 23);
}
```

1. The first number represents the amount of red,
2. the second is green,
3. and the third is blue.

In general, hex and RGB color representations are equivalent. Which you choose is a matter of personal taste. That said, it’s good to choose one and be consistent throughout your CSS, because it’s easier to compare hex to hex and RGB to RGB.

### Hex and RGB

The hexadecimal and rgb color system can represent many more colors than the small set of CSS named colors.

In both hex and RGB, we have three values, one for each color. Each can be one of 256 values. Specifically, `256 * 256 * 256 = 16,777,216`. That is the amount of colors we can now represent. Compare that to the roughly 140 named CSS colors

### Hue, Saturation, and Lightness

There’s another equally powerful system in CSS called the hue-saturation-lightness color scheme, abbreviated as`HSL`.

```css
color: hsl(120, 60%, 70%);
```

1. The first number represents the degree of the hue, and can be between 0 and 360.
2. The second and third numbers are percentages representing saturation and lightness respectively.

![Color wheel](/Notes/notes/01-HTML-CSS/assets/color_wheel_4_background.svg)

- Hue is the first number. It refers to an angle on a color wheel

1. Red is 0 degrees,
2. Green is 120 degrees,
3. Blue is 240 degrees,
4. and then back to Red at 360

- Saturation refers to the intensity or purity of the color. The saturation increases towards 100% as the color becomes richer. The saturation decreases towards 0% as the color becomes grayer.

- Lightness refers to how light or dark the color is. Halfway, or 50%, is normal lightness

- Sliding the dimmer up towards 100% makes the color lighter,

`HSL` is convenient for adjusting colors.
In `RGB`, making the color a little darker may affect all three color components.
In HSL, that’s as easy as changing the lightness value. HSL is also useful for making a set of colors that work well together by selecting various colors that have the same lightness and saturation but different hues.

### Opacity and Alpha

All of the colors we’ve seen so far have been opaque, or non-transparent.
When we overlap two opaque elements, nothing from the bottom element shows through the top element.

#### HSL

To use opacity in the HSL color scheme, use hsla instead of hsl, and four values instead of three. For example:

```css
color: hsla(34, 100%, 50%, 0.1);
```

The first three values work the same as hsl. The fourth value (which we have not seen before) is the alpha. This last value is sometimes called opacity.

- Alpha is a decimal number from zero to one.
- If alpha is zero, the color will be completely transparent.
- If alpha is one, the color will be opaque. The value for half-transparent would be 0.5.

#### RGB

The RGB color scheme has a similar syntax for opacity, rgba. Again, the first three values work the same as rgb and the last value is the alpha. Here’s an example:

```Css
color: rgba(234, 45, 98, 0.33);
```

Alpha can only be used with HSL, RGB, and hex colors; we cannot add the alpha value to name colors like green.

There is, however, a named color keyword for zero opacity, transparent. It’s equivalent to rgba(0, 0, 0, 0), and it’s used like any other color keyword:

```css
color: transparent;
```
