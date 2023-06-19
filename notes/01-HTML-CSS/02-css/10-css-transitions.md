# CSS TRANSITIONS

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)
  - [Duration](#duration)
  - [Timing Function](#timing-function)
  - [Delay](#delay)
  - [Shorthand](#shorthand)
  - [Combination](#combination)
  - [All](#all)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

- Learn CSS Grid to create grid layouts

## Notes

_tldr_:

CSS Transitions have 4 components:

- A property that will transition.
- The duration which describes how long the transition takes.
- The delay to pause before the transition will take place.
- The timing function that describes the transition’s acceleration

### Duration

**[⬆ back to top](#table-of-contents)**

To create a simple transition in CSS, we must specify two of the four aspects:

1. The property that we want to transition
2. The duration of the transition.

In the example above,

- transition-property `declares which CSS property` we will be transitioning : the text color.

- transition-duration, `declares how long the transition will take` : one second.

- All CSS properties are animatable unless otherwise specified, [see this resource](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_animated_properties).The type of transition depends on the property you choose. (`Background-image is not animatable`.)

- Different properties transition in different ways, for example:

- Color values, like color and background-color, will blend to a new color
- Length values like font-size, width, and height will grow or shrink.

- Duration is specified in seconds or milliseconds, such as 3s, 0.75s, 500ms. The default value is 0s, or instantaneous, as if there is no transition.

💡 `Tip`:

When choosing a duration, think about how long actions take in real life. For example, a human eye blink takes around 400ms. People might expect the animation of clicking a button to be as sudden as a blink.

### Timing Function

**[⬆ back to top](#table-of-contents)**

We learned two of the four aspects of transition in CSS. The next transition property is `transition-timing-function`.

- The timing function `describes the pace of the transition`.
- The default value is ease, which :

1. starts the transition slowly
2. speeds up in the middle
3. slows down again at the end.

Other valid values include:

- `ease-in` - starts slow, accelerates quickly, stops abruptly.
- `ease-out` — begins abruptly, slows down, and ends slowly.
- `ease-in-out` — starts slow, gets fast in the middle, and ends slowly.
- `linear` — constant speed throughout.

```css
transition-property: color;
transition-duration: 1s;
transition-timing-function: ease-out;
```

In the example above,

- the text color will be animated over one second.
- The timing function is ease-out which means it will begin abruptly and slow down as it ends.

### Delay

**[⬆ back to top](#table-of-contents)**

The last transition property is `transition-delay`. Much like duration, its value is an amount of time. Delay specifies the time to wait before starting the transition.
As with the duration property, the default value for transition-delay is 0s.

```css
transition-property: width;
transition-duration: 750ms;
transition-delay: 250ms;
```

In the example above, a change in the width of the element

- will start after a quarter of a second.
- and it will animate over three quarters of a second.

### Shorthand

**[⬆ back to top](#table-of-contents)**

Because these four properties are so frequently declared together, CSS provides a property that can be used to declare them all in one line: `transition`.

The properties must be specified in this order:

1. transition-property
2. transition-duration,
3. transition-timing-function,
4. transition-delay.

```css
transition: color 1.5s linear 0.5s;
```

This example

- will cause any change in text color
- to transition at constant speed over 1.5 seconds,
- after a delay of 0.5 seconds.

**Leaving out one of the properties causes the default value for that property to be applied.**
There is one exception: You must set duration if you want to define delay. Since both are time values, the browser will always interpret the first time value it sees as duration.

### Combination

**[⬆ back to top](#table-of-contents)**

The shorthand transition rule has one advantage over the set of separate `transition-property>` rules:

- you can describe unique transitions for multiple properties, and combine them.

- add a comma (,) to combine the properties

```css
transition: color 1s linear, font-size 750ms ease-in 100ms;
```

- The above code transitions two properties at once :

1. The text color transitions over one second with linear timing and no delay.

2. At the same time, the font size transitions over 750 milliseconds with an ease-in timing and a 100 millisecond delay.

### All

**[⬆ back to top](#table-of-contents)**

It is common to use the same duration, timing function, and delay for multiple properties. When this is the case you can set the transition-property value to all. This will apply the same values to all properties. To effect this, you can use `all` as a value for transition-property.

- all means every value that changes will be transitioned in the same way.
- This allows you to describe the transition of many properties with a single line

```css
transition: all 1.5s linear 0.5s;
```

In this example, : any change will be animated over one and a half seconds after a half-second delay with linear timing.

```css
.button span,
.button div,
.button i {
  transition: all 1.2s ease-out 0.2s;
}
```
