# GRID FUNDAMENTALS

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)
  - [Creating a Grid](#creating-a-grid)
  - [Creating columns](#creating-columns)
  - [Creating rows](#creating-rows)
  - [Grid Template](#grid-template)
  - [Fraction](#fraction)
  - [Repeat](#repeat)
  - [MinMax](#minmax)
  - [Grid Gap](#grid-gap)
  - [Multiple Row Items](#multiple-row-items)
  - [Grid Row](#grid-row)
  - [Grid Column](#grid-column)
  - [Grid Area](#grid-area)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

- Learn CSS Grid to create grid layouts

## Notes

_tldr_:

1. grid-template-columns defines the number and sizes of the columns of the grid

2. grid-template-rows defines the number and sizes of the rows of the grid

3. grid-template is a shorthand for defining both grid-template-columns and grid-template-rows in one line

4. row-gap puts blank space between the rows of the grid

5. column-gap puts blank space between the columns of the grid

6. gap is a shorthand for defining both row-gap and column-gap in one line

7. grid-row-start and grid-row-end makes elements span certain rows of the grid

8. grid-column-start and grid-column-end makes elements span certain columns of the grid

9. grid-area is a shorthand for grid-row-start, grid-column-start, grid-row-end, and. grid-column-end, all in one line

### Creating a Grid

**[⬆ back to top](#table-of-contents)**

To set up a grid :

- you need to have both a grid container and grid items
- The grid container will be a parent element that contains grid items.
- and applies styling and positioning to them.

To turn an HTML element into a grid container:

- you must set the element’s display property to one of two values:

1. `grid` — for a block-level grid.
2. `inline-grid` — for an inline grid

Then, you can assign other properties to lay out the grid to suit your needs.

```css
.grid {
  display: grid;
}
/* or */
.grid {
  display: inline-grid;
}
```

### Creating Columns

**[⬆ back to top](#table-of-contents)**

- grids contain only one column by default, so if you were to start adding items, each item would be put on a new row;

- To change this, we need to explicitly define the number of rows and columns in our grid.

We can define the columns

- by using the CSS property grid-template-columns

```css
.grid {
  display: grid;
  width: 500px;
  grid-template-columns: 100px 200px;
}
```

This property creates two changes :

1. it defines the number of columns in the grid; in this case, there are two (`100px`) - (`200px`)

2. it sets the width of each column. The first will be 100 pixels wide and the second will be 200 pixels wide

We can also define the size of our columns as a `percentage` of the entire grid’s width.

```css
.grid {
  display: grid;
  width: 1000px;
  grid-template-columns: 20% 50%;
}
```

In this example:

- the grid (the container) is `1000` pixels wide.
- the first column will be 200 pixels wide because it is set to be 20% of the grid’s width.
- The second column will be 500 pixels wide.

We can also mix and match these two units. In the example below, there are three columns of width 20 pixels, 40 percent, and 60 pixels:

```css
.grid {
  display: grid;
  width: 100px;
  grid-template-columns: 20px 40% 60px;
}
```

Notice that in this example, the total width of our columns (120 pixels -> 40% \* 100px = 40px) exceeds the width of the grid (100 pixels). This might make our grid cover other elements on the page! In a later exercise, we will discuss how to avoid overflow.

### Creating Rows

**[⬆ back to top](#table-of-contents)**

By default, the rows are sized to fit evenly inside the grid. We can change the size of our rows.

To specify the `number` and `size` of the rows, we are going to use the property `grid-template-rows`.

```css
.grid {
  display: grid;
  width: 1000px;
  height: 500px;
  grid-template-columns: 100px 200px;
  grid-template-rows: 10% 20% 600px;
}
```

- This grid has two columns and three rows
- grid-template-rows defines the `number of rows` and sets each `row’s height`.
- the first row is `50 pixels tall` (10% of 500),
- the second row is `100 pixels` tall (20% of 500)
- the third row is 600 pixels tall.

**Note: Remember that rows are defined as a percentage of the grid’s height, and columns are defined as a percentage of its width.**

### Grid Template

**[⬆ back to top](#table-of-contents)**

The shorthand property, `grid-template` can replace `grid-template-rows` and `grid-template-columns`.

```css
.grid {
  display: grid;
  width: 1000px;
  height: 500px;
  grid-template: 200px 300px / 20% 10% 70%;
}
```

- the values before the slash will determine the size of each row (200px 300px)
- The values after the slash determine the size of each column.
- we’ve made two rows and three columns of varying sizes.

### Fraction

**[⬆ back to top](#table-of-contents)**

You may already be familiar with several types of responsive units such as

- percentages (%),
- ems and rems.

CSS Grid introduced a new relative sizing unit — `fr`, like fraction.

By using the `fr` unit,

- We can define `the size of columns and rows as a fraction of the grid’s length and width`
- This unit was specifically created for use in CSS Grid.
- Using `fr` makes it easier to `prevent grid items from overflowing the boundaries of the grid`

```css
.grid {
  display: grid;
  width: 1000px;
  height: 400px;
  grid-template: 2fr 1fr 1fr / 1fr 3fr 1fr;
}
```

In this example :
`rows`

- the grid will have three rows and three columns.
- The rows are splitting up the available 400 pixels of height into four parts (2fr + 1fr + 1fr = 4fr so `four parts`).
- The first row gets two of those parts
- the second and the third row get one
- Therefore the first row is `200 pixels tall(400 * 2/4 )`, and the second and third rows are `100 pixels tall(400 * 1/4 )`.

`columns`

- Each column’s width is a fraction of the available space.
- In this case, the available space is split into five parts (1fr + 3fr + 1fr = 5fr so `five parts`).
- The first column gets one-fifth of the space.
- the second column gets three-fifths,
- the last column gets one-fifth.
- Since the total width is 1000 pixels, this means that the columns will have widths of `200 pixels (1000px * 1/5)`, `600 pixels (1000px * 3/5)`, and `200 pixels(1000px * 1/5)` respectively.

It is possible to use fr with other units as well. When this happens, each fr represents a fraction of the available space.

```css
.grid {
  display: grid;
  width: 100px;
  grid-template-columns: 1fr 60px 1fr;
}
```

- 60 pixels are taken up by the second column
- Therefore the first and third columns have 40 available to split between them.
- Since each gets one fraction of the total, they both end up being 20 pixels wide.

### Repeat

**[⬆ back to top](#table-of-contents)**

The properties that define the number of rows and columns can take a function as a value.

- `repeat()` is one of these functions.

```css
.grid {
  display: grid;
  width: 300px;
  grid-template-columns: repeat(3, 100px);
}
```

The repeat function will :

- duplicate the specifications for rows or columns a given number of times.

In the example above, using the repeat function

- will make the grid have three columns that are each 100 pixels wide.

It is the same as writing:

```css
grid-template-columns: 100px 100px 100px;
```

Repeat is particularly useful with fr.

- For example, repeat(5, 1fr) would split your table into five equal rows or columns.

Finally, the second parameter of repeat() can have multiple values.

```css
grid-template-columns: repeat(2, 20px 50px);
```

This code will create four columns where

- the first and third columns will be 20 pixels wide
- and the second and fourth will be 50 pixels wide.

It is the same as writing:

```css
grid-template-columns: 20px 50px 20px 50px;
```

### MinMax

**[⬆ back to top](#table-of-contents)**

So far, all of the grids that we have worked with have been a fixed size.
The grid in our example has been 400 pixels wide and 500 pixels tall.
But sometimes

- you might want a grid to resize based on the size of your web browser.
- you might want to prevent a row or column from getting too big or too small.
  For example,

- if you have a 100-pixel wide image in your grid,
- you probably don’t want its column to get thinner than 100 pixels!
- The `minmax()` function can help us solve this problem.

```css
.grid {
  display: grid;
  grid-template-columns: 100px minmax(100px, 500px) 100px;
}
```

In this example:

- the first and third columns will always be 100 pixels wide, no matter the size of the grid.
- The second column, however, will vary in size as the overall grid resizes.
- The second column will always be between 100 and 500 pixels wide.

To see minmax() in action, we need to first make the grid have a variable width so that the grid resizes with the window.

### Grid Gap

**[⬆ back to top](#table-of-contents)**

In all of our grids so far, there hasn’t been any space between the items in our grid.
The CSS properties `row-gap` and `column-gap` :

- will put blank space between every row and column in the grid.

```css
.grid {
  display: grid;
  width: 320px;
  grid-template-columns: repeat(3, 1fr);
  column-gap: 10px;
}
```

- It is important to note that grid gap properties do not add space at the beginning or end of the grid
- our grid will have three columns `repeat(3, 1fr)` with `two ten-pixel gaps` between them.

Let’s quickly calculate how wide these columns are.

- Using fr considers all of the available space.
- The grid is 320 pixels wide and 20 of those pixels are taken up by the two grid gaps(10px +10px = 20px).
- Therefore each column takes a piece of the 300 available pixels
- Each column gets 1fr, so the columns are evenly divided into thirds (or 100 pixels each)

there is a shorthand CSS property, gap, that can set the row and column gap at the same time

```css
.grid {
  display: grid;
  width: 320px;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px 10px;
}
```

The example above :

- will set the distance between rows to 20 pixels
- the distance between columns to 10 pixels
- If only one value is given, it will set the column gap and the row gap to that value.

### Grid Items

**[⬆ back to top](#table-of-contents)**

![Grid Diagram](/Notes/notes/01-HTML-CSS/assets/css_grid_diagram_2.svg)

In all of our examples, the items placed in the grid have always taken up exactly one square.
This does not always need to be the case; we can drastically change the look of our grid by making grid items take up more than one row and one column.
You can see this in the diagram :

- Items A, B, C, and E span more than one row!

We will learn CSS properties that will affect the size of the grid items and where they are displayed on the page

### Multiple Row Items

**[⬆ back to top](#table-of-contents)**

Using the CSS properties `grid-row-start` and `grid-row-end`, we can make single grid items take up multiple rows.

**we are no longer applying CSS to the outer grid container.** we’re adding CSS to the elements sitting inside the grid!

```css
.item {
  grid-row-start: 1;
  grid-row-end: 3;
}
```

In the example above

- The HTML element of class item will `take up two rows in the grid, rows 1 and 2`.
- The values that grid-row-start and grid-row-end accept are `grid lines`.
- Row and column grid lines start at 1 and end at a value that is 1 greater than the number of rows or columns the grid has.

For example,

- if a grid has 5 rows, the grid row lines range from 1 to 6.
- If a grid has 8 rows, the grid row lines range from 1 to 9.

- The value for grid-row-start should be the row at which you want the grid item to begin.
- The value for grid-row-end should be one greater than the row at which you want the grid item to end.

For example, An element that covers rows 2, 3, and 4 should have these declarations :

- `grid-row-start: 2` -> the row at which you want the grid item to begin
- `grid-row-end: 5`. -> one greater than the row at which you want the grid item to end (4 + 1 =5)

### Grid Row

**[⬆ back to top](#table-of-contents)**

We can use the property grid-row as shorthand for grid-row-start and grid-row-end

```css
.item {
  grid-row-start: 4;
  grid-row-end: 6;
}
```

It's the same as writting :

```css
.item {
  grid-row: 4 / 6;
}
```

- In this case, the starting row goes before the `/`
- The ending row goes after it.
- the ending row is exclusive; this grid item will occupy rows four and five.

When an item spans multiple rows or columns using these properties:

- it will also include the gap if any exists.
- if an item spans two rows of height 100 pixels and there is a ten-pixel gap, then the item will have a total height of 210 pixels.

### Grid Column

**[⬆ back to top](#table-of-contents)**

The previous three properties also exist for columns.`grid-column-start`, `grid-column-end` and `grid-column` work identically to the row properties. These properties allow a grid item to span multiple columns.

- When using these properties, we can use the keyword `span` to start or end a column or row, relative to its other end.

```css
.item {
  grid-column: 4 / span 2;
}
```

- This is telling the item element to begin in column four
- and take up two columns of space
- So item would occupy columns four and five.

It's the same as writting :

```css
.item {
  grid-column: 4 / 6;
}
```

```css
.item {
  grid-column-start: 4;
  grid-column-end: span 2;
}
```

```css
.item {
  grid-column-start: span 2;
  grid-column-end: 6;
}
```

### Grid Area

**[⬆ back to top](#table-of-contents)**

We’ve already been able to use `grid-row` and `grid-column` as shorthand for properties like `grid-row-start` and `grid-row-end`. We can refactor even more using the property `grid-area`.
This property will set the starting and ending positions for both the rows and columns of an item.

```css
.item {
  grid-area: 2 / 3 / 4 / span 5;
}
```

- grid-area takes four values separated by slashes
- The order is important!
- `grid-row-start` / `grid-column-start` / `grid-row-end` / `grid-column-end`

In the above example,

- the item will start on row 2 and end on row 4
- The item will then start on column 3 and span 5 columns
- this means the item will include columns 3, 4, 5, 6, and 7
