# HTML FORMS

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)

  - [Introduction to HTML Forms](#notes)

    - [How A Form works?](#how-a-form-works)
    - [Text Input](#text-input)
    - [Adding a label](#adding-a-label)
    - [Password input](#password-input)
    - [Number input](#number-input)
    - [Range input](#range-input)
    - [Checkbox input](#checkbox-input)
    - [Radio Button input](#radio-button-input)
    - [Radio Button input](#radio-button-input)
    - [Dropdown list](#dropdown-list)
    - [Datalist Input](#datalist-input)
    - [Textarea element](#textarea-element)
    - [Submit form](#submit-form)

  - [Form Validation](#form-validation)

    - [Introduction to HTML Form Validation](#introduction-to-html-form-validation)
    - [Requiring an Input](#requiring-an-input)
    - [Set a Minimum and Maximum](#set-a-minimum-and-maximum)
    - [Checking Text Length](#checking-text-length)
    - [Matching a Pattern](#matching-a-pattern)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

- Learning the structure and syntax of a `<form>` and the many elements that populate it.

## Notes

_TLDR_:

- The purpose of a `<form>` is to allow users to input information and send it.
- The `<form>`‘s action attribute determines where the form’s information goes.
- The `<form>`‘s method attribute determines how the information is sent and processed.
- To add fields for users to input information we use the `<input>` element and set the type attribute to a field of our choosing:
- Setting type to "text" creates a single row field for text input.
- Setting type to "password" creates a single row field that censors text input.
- Setting type to "number" creates a single row field for number input.
- Setting type to "range" creates a slider to select from a range of numbers.
- Setting type to "checkbox" creates a single checkbox that can be paired with other checkboxes.
- Setting type to "radio" creates a radio button that can be paired with other radio buttons.
- Setting type to "text" and adding the list attribute will pair the `<input>` with a `<datalist>` element if the list of `<input>` and the id of `<datalist>` are the same.
- Setting type to "submit" creates a submit button.
- A `<select>` element is populated with `<option>` elements and renders a dropdown list selection.
- A `<datalist>` element is populated with `<option>` elements and works with an `<input>` to search through choices.
- A `<textarea>` element is a text input field that has a customizable area.
- When a `<form>` is submitted, the name of the fields that accept input and the value of those fields are sent as name=value pairs.

### Introduction to HTML Forms

**[⬆ back to top](#table-of-contents)**

![Search Bar](/notes/01-HTML-CSS/assets/search-bar.gif)

Just like a physical form, an HTML `<form>` element is responsible for `collecting information to send somewhere else`.

### How A Form works?

**[⬆ back to top](#table-of-contents)**

The `<form>` element is a great tool for `collecting information`, but then we need to send that information somewhere else for processing. We need to supply the `<form>` element with both the `location of where the <form>‘s information goes` and `what HTTP request to make`.

```html
<form action="/example.html" method="POST"></form>
```

In the above example, we’ve created the skeleton for a `<form>` that will send information to `example.html` as a `POST request`:

- The action attribute determines where the information is sent.
- The method attribute is assigned a HTTP verb that is included in the HTTP request

### Text Input

**[⬆ back to top](#table-of-contents)**

The `<input>` element has a `type attribute` which determines `how it renders` on the web page and `what kind of data` it can accept.

- When we create an `<input>` element with `type="text"`, it renders a text field that users can type into.

- It’s also important that we include a name attribute for the `<input>`— without the name attribute, information in the `<input>`won’t be sent when the`<form>` is submitted.

- After users type into the `<input>` element, the value of the `value attribute` becomes what is typed into the text field.

- The value of the value attribute is paired with the value of the name attribute and sent as text when the form is submitted.

For instance, if a user typed in “important details” in the text field `<input>` element:
When the form is submitted, the text: `"first-text-field=important details"` is sent to /`example.html`

```html
<form action="/example.html" method="POST">
  <input type="text" name="first-text-field" value="already pre-filled" />
</form>
```

- We could also assign a default value for the value attribute so that users have a pre-filled text field when they first see the rendered form like we did above.

### Adding a label

**[⬆ back to top](#table-of-contents)**

- For a user `to properly identify an <input> we use the appropriately named <label> element`

- To associate a `<label>` and an `<input>`, the `<input>` needs an `id attribute`. We then assign the`for` attribute of the `<label>` element with the value of the `id` attribute of `<input>` :

```html
<form action="/example.html" method="POST">
  <label for="meal">What do you want to eat?</label>
  <br />
  <input type="text" name="food" id="meal" />
</form>
```

![Input with label](/notes/01-HTML-CSS/assets/label-text-input.webp)

- Another benefit for using the `<label>` element is when this element is clicked, the corresponding `<input>` is highlighted/selected.

### Password input

**[⬆ back to top](#table-of-contents)**

- An `<input type ="password">` element will replace input text with another character like an asterisk (\*) or a dot (•).

```html
<form>
  <label for="user-password">Password: </label>
  <input type="password" id="user-password" name="user-password" />
</form>
```

After a user types into the field, it would look like:

![Input with password](/notes/01-HTML-CSS/assets/pwInput-labeled-filled.webp)

- Even though the password field obscures the text of the password, when the form is submitted, the value of the text is sent. In other words, if “hunter2” is typed into the password field, “user-password=hunter2” is sent along with the other information on the form.

### Number input

**[⬆ back to top](#table-of-contents)**

- By setting `type="number"` for an `<input>` we can restrict what users type into the input field to just numbers (and a few special characters like -, +, and .)

- We can also provide a step attribute which creates arrows inside the input field to increase or decrease by the value of the step attribute

```html
<form>
  <label for="years"> Years of experience: </label>
  <input id="years" name="years" type="number" step="1" />
</form>
```

renders :

![Number input](/notes/01-HTML-CSS/assets/numInput-labeled.webp)

### Range input

**[⬆ back to top](#table-of-contents)**

- When we wanted to limit what numbers our users could type we might consider, we could use the type "range" which creates a slider.

- To set the minimum and maximum values of the slider we assign values to the min and max attribute of the `<input>`

- We could also control how smooth and fluid the slider works by assigning the step attribute a value.

```html
<form>
  <label for="volume"> Volume Control</label>
  <input id="volume" name="volume" type="range" min="0" max="100" step="1" />
</form>
```

![Range input](/notes/01-HTML-CSS/assets/rangeInput-labeled.webp)

### Checkbox input

**[⬆ back to top](#table-of-contents)**

When we want to present multiple options to users and allow them to select any number of options.

```html
<form>
  <p>Choose your pizza toppings:</p>
  <label for="cheese">Extra cheese</label>
  <input id="cheese" name="topping" type="checkbox" value="cheese" />
  <br />
  <label for="pepperoni">Pepperoni</label>
  <input id="pepperoni" name="topping" type="checkbox" value="pepperoni" />
  <br />
  <label for="anchovy">Anchovy</label>
  <input id="anchovy" name="topping" type="checkbox" value="anchovy" />
</form>
```

Which renders:

![Range input](/notes/01-HTML-CSS/assets/checkboxInput-labeled.webp)

### Radio Button input

**[⬆ back to top](#table-of-contents)**

- there are cases where we want to present multiple options and only allow for one selection like asking users if they agree or disagree with the terms and conditions.

```html
<form>
  <p>What is sum of 1 + 1?</p>
  <input type="radio" id="two" name="answer" value="2" />
  <label for="two">2</label>
  <br />
  <input type="radio" id="eleven" name="answer" value="11" />
  <label for="eleven">11</label>
</form>
```

![Range input](/notes/01-HTML-CSS/assets/radioInput-labeled.webp)

- Notice from the code snippet, radio buttons (like checkboxes) do not display their value. We have an associated `<label>` to represent the value of the radio button

- To group radio buttons together, we assign them the same name and only one radio button from that group can be selected.

### Dropdown list

**[⬆ back to top](#table-of-contents)**

When we have a whole list of options to choose from, one solution is to use a dropdown list to allow our users to choose one option from an organized list.

```html
<form>
  <label for="lunch">What's for lunch?</label>
  <select id="lunch" name="lunch">
    <option value="pizza">Pizza</option>
    <option value="curry">Curry</option>
    <option value="salad">Salad</option>
    <option value="ramen">Ramen</option>
    <option value="tacos">Tacos</option>
  </select>
</form>
```

Which renders:

![Dropdown list](/notes/01-HTML-CSS/assets/dropdown-list-concealed.webp)

And if we click on the field containing the first option, the list is revealed :

![Dropdown list](/notes/01-HTML-CSS/assets/dropdown-list-revealed.webp)

When the `<form>` is submitted, the information from this input field will be sent using the name of the `<select>` and the value of the chosen `<option>`. For instance, if a user selected Pizza from the dropdown list, the information would be sent as `"lunch=pizza"`.

### Datalist Input

**[⬆ back to top](#table-of-contents)**

- if the list has a lot of options, it could be tedious for users to scroll through the entire list to locate one option. That’s where using the `<datalist>` element comes in handy

- The `<datalist>` is used with an `<input type="text">` element. The `<input>` creates a text field that users can type into and filter options from the `<datalist>`.

- To associate a `<datalist>` element with the `<input>` element, the `<input>`‘s list attribute value must match the id of the `<datalist>` element

```html
<form>
  <label for="city">Ideal city to visit?</label>
  <input type="text" list="cities" id="city" name="city" />

  <datalist id="cities">
    <option value="New York City"></option>
    <option value="Tokyo"></option>
    <option value="Barcelona"></option>
    <option value="Mexico City"></option>
    <option value="Melbourne"></option>
    <option value="Other"></option>
  </datalist>
</form>
```

- Notice, in the code above, we have an `<input>` that has a list attribute. The `<input>` is associated to the `<datalist>` via the `<input>`‘s list attribute and the id of the `<datalist>`.

From the code provided, the following form is rendered:
![Datalist revealed](/notes/01-HTML-CSS/assets/datalist-concealed.webp)

And when field is selected:
![Datalist revealed](/notes/01-HTML-CSS/assets/datalist-revealed.webp)

- there are some major differences between `<select>` and `<datalist>`. In the associated `<input>` element, users can type in the input field to search for a particular option. If none of the `<option>` s match, the user can still use what they typed in. When the form is submitted, the value of the `<input>` ‘s name and the value of the option selected, or what the user typed in, is sent as a pair.

### Textarea element

**[⬆ back to top](#table-of-contents)**

- There are cases where users need to write in more information, like a blog post. In such cases, instead of using an `<input>`, we could use `<textarea>`.

- The `<textarea>` element is used to create a bigger text field for users to write more text.

- We can add the attributes rows and cols to determine the amount of rows and columns for the `<textarea>`

```html
<form>
  <label for="blog">New Blog Post: </label>
  <br />
  <textarea id="blog" name="blog" rows="5" cols="30"> </textarea>
</form>
```

an empty `<textarea>` that is 5 rows by 30 columns is rendered to the page like so:

![Textarea](/notes/01-HTML-CSS/assets/textarea-blank.webp)

- When we submit the form, the value of `<textarea>` is the text written inside the box.
- If we want to add a default value to `<textarea>` we would include it within the opening and closing tags.

```html
<textarea>Adding default text</textarea>
```

### Submit form

**[⬆ back to top](#table-of-contents)**

- The purpose of a form is to collect information that will be submitted. That’s the role of the submit button — users click on it when they are finished with filling out information in the `<form>` and they’re ready to send it off

- To make a submit button in a `<form>`, we’re going to use the reliable`<input>` element and set the type to "submit". For instance:

```html
<form>
  <input type="submit" value="Send" />
</form>
```

![submit](/notes/01-HTML-CSS/assets/submit-button2.webp)

### Form validation

**[⬆ back to top](#table-of-contents)**

_TLDR_:

- Client-side validations happen in the browser before information is sent to a server.

- Adding the required attribute to an input related element will validate that the input field has information in it.

- Assigning a value to the min attribute of a number input element will validate an acceptable minimum value.

- Assigning a value to the max attribute of a number input element will validate an acceptable maximum value.

- Assigning a value to the minlength attribute of a text input element will validate an acceptable minimum number of characters.

- Assigning a value to the maxlength attribute of a text input element will validate an acceptable maximum number of characters.

- Assigning a regex to pattern matches the input to the provided regex.

- If validations on a `<form>` do not pass, the user gets a message explaining why and the `<form>` cannot be submitted.

#### Introduction to HTML Form Validation

**[⬆ back to top](#table-of-contents)**

![HTML Form Validation](/notes/01-HTML-CSS/assets/form-validation.gif)

- Validation is the concept of checking user provided data against the required data.

There are different types of validation.

- One type is `server-side validation`, this happens when data is sent to another machine (typically a server) for validation.
  -> An example of this type of validation is the usage of a login page. The form on the login page accepts username and password input, then sends the data to a server that checks that the pair matches up correctly

- We use `client-side validation` if we want to check the data on the browser (the client). This validation occurs before data is sent to the server.

#### Requiring an Input

**[⬆ back to top](#table-of-contents)**

- Sometimes we have fields in our `<form>` s which are not optional, i.e. there must be information provided before we can submit it

- To enforce this rule, we can add the required attribute to an `<input>` element.

```html
<form action="/example.html" method="POST">
  <label for="allergies">Do you have any dietary restrictions?</label>
  <br />
  <input id="allergies" name="allergies" type="text" required />
  <br />
  <input type="submit" value="Submit" />
</form>
```

This renders a text box, and if we try to submit the`<form>` without filling it out we get this message:

![Required field](/notes/01-HTML-CSS/assets/required-field.webp)

- The styling of the message varies from browser to browser, the picture above depicts the message in a Chrome browser.

#### Set a Minimum and Maximum

**[⬆ back to top](#table-of-contents)**

- Another built-in validation we can use is to assign a minimum or maximum value for a number field, e.g. `<input type="number">` and `<input type="range">`

- To set a minimum acceptable value, we use the min attribute and assign a value.
- On the flip side, to set a maximum acceptable value, we assign the max

```html
<form action="/example.html" method="POST">
  <label for="guests">Enter # of guests:</label>
  <input id="guests" name="guests" type="number" min="1" max="4" />
  <input type="submit" value="Submit" />
</form>
```

If a user tries to submit an input that is less than 1 a warning will appear:

![Min-Max](/notes/01-HTML-CSS/assets/min-max.webp)

#### Checking Text Length

**[⬆ back to top](#table-of-contents)**

- There are certainly cases where we wouldn’t want our users typing more than a certain number of characters (think about the character cap for messages on Twitter).

- We might even want to set a minimum number of characters. Conveniently, there are built-in HTML5 validations for these situations.

- To set a minimum number of characters for a text field, we add the `minlength` attribute and a value to set a minimum value.

- To set the maximum number of characters for a text field, we use the `maxlength` attribute and set a maximum value

```html
<form action="/example.html" method="POST">
  <label for="summary"
    >Summarize your feelings in less than 250 characters</label
  >
  <input
    id="summary"
    name="summary"
    type="text"
    minlength="5"
    maxlength="250"
    required
  />
  <input type="submit" value="Submit" />
</form>
```

![Minlength](/notes/01-HTML-CSS/assets/minlength.webp)

#### Matching a Pattern

**[⬆ back to top](#table-of-contents)**

- We could also add a validation to check how the text was provided.

- For cases when we want user input to follow specific guidelines, we use the pattern attribute and assign it a regular expression, or regex.

- Regular expressions are a sequence of characters that make up a search pattern. If the input matches the regex, the form can be submitted.

- Let’s say we wanted to check for a valid credit card number (a 14 to 16 digit number). We could use the regex: [0-9]{14,16} which checks that the user provided only numbers and that they entered at least 14 digits and at most 16 digits.

```html
<form action="/example.html" method="POST">
  <label for="payment">Credit Card Number (no spaces):</label>
  <br />
  <input
    id="payment"
    name="payment"
    type="text"
    required
    pattern="[0-9]{14,16}"
  />
  <input type="submit" value="Submit" />
</form>
```

- With the pattern in place, users can’t submit the `<form>` with a number that doesn’t follow the regex. When they try, they’ll see a validation message like so:

![Pattern](/notes/01-HTML-CSS/assets/pattern.webp)
