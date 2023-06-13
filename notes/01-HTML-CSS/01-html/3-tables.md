# HTML DOCUMENT STANDARD

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)
  - [Create a Table](#create-a-table)
  - [Table Rows](#table-rows)
  - [Table Data](#table-data)
  - [Table Headings](#table-headings)
  - [Table Borders](#table-borders)
  - [Spanning Columns](#spanning-rows)
  - [Table Body, Table Head, Table footer](#table-body-table-head-table-footer)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

- Learning how to use the HTML `<table>` element to present information in a two-dimensional table to the users.

## Notes

_TLDR_:

- The `<table>` element creates a table.
- The `<tr>` element adds rows to a table.
- To add data to a row, you can use the `<td>` element.
- Table headings clarify the meaning of data. Headings are added with the `<th>` element.
- Table data can span columns using the colspan attribute.
- Table data can span rows using the rowspan attribute.
- Tables can be split into three main sections: a head, a body, and a footer.
- A table’s head is created with the `<thead>` element.
- A table’s body is created with the `<tbody>` element.
- A table’s footer is created with the `<tfoot>` element.
- All the CSS properties you learned about in this course can be applied to tables and their data.

### Create a Table

**[⬆ back to top](#table-of-contents)**

Before displaying data, we must first create the table that will contain the data by using the `<table>` element.

```html
<table></table>
```

### Table Rows

**[⬆ back to top](#table-of-contents)**

The first step in entering data into the table is to add rows using the table row element: `<tr>`.

```html
<table>
  <tr></tr>
  <tr></tr>
</table>
```

In the example above, two rows have been added to the table.

### Table Data

**[⬆ back to top](#table-of-contents)**

Each cell element must also be defined. In HTML, you can add data using the table data element: `<td>`

```html
<table>
  <tr>
    <td>73</td>
    <td>81</td>
  </tr>
</table>
```

In the example above, two data points (73 and 81) were entered in the one row that exists. By adding two data points, we created two cells of data

### `Table Headings`

**[⬆ back to top](#table-of-contents)**

Table data doesn’t make much sense without titles to describe what the data represents.

To add titles to rows and columns, you can use the table heading element: `<th>`.

```html
<table>
  <tr>
    <th></th>
    <th scope="col">Saturday</th>
    <th scope="col">Sunday</th>
  </tr>
  <tr>
    <th scope="row">Temperature</th>
    <td>73</td>
    <td>81</td>
  </tr>
</table>
```

- First, a new row was added to hold the three headings: a blank heading, a Saturday heading, and a Sunday heading.

- In the second row, one table heading was added as a row title: Temperature.

When rendered, this code will appear similar to :
| | Saturday | Sunday |
| ---------------- | ------------------------------- | ----------------------------- |
| Temperature | 73 | 81 |

**Note also, the use of the scope attribute, which can take one of two values.**

- `row` - this value makes it clear that the heading is for a row.

- `col` this value makes it clear that the heading is for a column.

### `Table Borders`

**[⬆ back to top](#table-of-contents)**

We use CSS to add style to HTML documents, because it helps us to separate the structure of a page from how it looks.

You can achieve the border effect using CSS.

```css
table,
td {
  border: 1px solid black;
}
```

### Spanning Columns

**[⬆ back to top](#table-of-contents)**

What if the table contains data that spans multiple columns?

For example, a personal calendar could have events that span across multiple hours, or even multiple days.

Data can span columns using the `colspan` attribute.

The attribute accepts an integer `(greater than or equal to 1)` to denote the number of columns it spans across.

```html
<table>
  <tr>
    <th>Monday</th>
    <th>Tuesday</th>
    <th>Wednesday</th>
  </tr>
  <tr>
    <td colspan="2">Out of Town</td>
    <td>Back in Town</td>
  </tr>
</table>
```

In the example above, the data `Out of Town` spans the `Monday and Tuesday` table headings using the value 2 (two columns). The data `Back in Town` appear only under the Wednesday heading.

### Spanning Rows

**[⬆ back to top](#table-of-contents)**

Data can also span multiple rows using the `rowspan` attribute.
(perhaps an event goes on for multiple hours on a certain day). It accepts an integer (greater than or equal to 1)

```html
<table>
  <tr>
    <!-- Row 1 -->
    <th></th>
    <th>Saturday</th>
    <th>Sunday</th>
  </tr>
  <tr>
    <!-- Row 2 -->
    <th>Morning</th>
    <td rowspan="2">Work</td>
    <td rowspan="3">Relax</td>
  </tr>
  <tr>
    <!-- Row 3 -->
    <th>Afternoon</th>
  </tr>
  <tr>
    <!-- Row 4 -->
    <th>Evening</th>
    <td>Dinner</td>
  </tr>
</table>
```

In the example above, there are four rows:

- The first row contains an empty cell and the two column headings.
- The second row contains the Morning row heading, along with Work, which spans two rows under the Saturday column. The “Relax” entry spans three rows under the Sunday column.
- The third row only contains the Afternoon row heading.
- The fourth row only contains the Dinner entry, since “Relax” spans into the cell next to it.

### `Table Body, Table Head, Table footer`

**[⬆ back to top](#table-of-contents)**

Long tables can be sectioned off using the table body element: `<tbody>`
The `<tbody>` element should contain all of the table’s data, excluding the table headings
it makes sense to section off the table’s column headings using the`<thead>` element.

```html
<table>
  <thead>
    <tr>
      <th></th>
      <th scope="col">Saturday</th>
      <th scope="col">Sunday</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Morning</th>
      <td rowspan="2">Work</td>
      <td rowspan="3">Relax</td>
    </tr>
    <tr>
      <th scope="row">Afternoon</th>
    </tr>
    <tr>
      <th scope="row">Evening</th>
      <td>Dinner</td>
    </tr>
  </tbody>
</table>
```

The table headings are contained inside of this element. Note that the table’s head still requires a row in order to contain the table headings. Additionally, only the column headings go under the `<thead>` element. We can use the scope attribute on `<th>` elements to indicate whether a `<th>` element is being used as a "row" heading or a "col" heading.

The bottom part of a long table can also be sectioned off using the `<tfoot>` element.
Footers are often used to contain sums, differences, and other data results
.

```html
<table>
  <thead>
    <tr>
      <th>Quarter</th>
      <th>Revenue</th>
      <th>Costs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Q1</th>
      <td>$10M</td>
      <td>$7.5M</td>
    </tr>
    <tr>
      <th>Q2</th>
      <td>$12M</td>
      <td>$5M</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <th>Total</th>
      <td>$22M</td>
      <td>$12.5M</td>
    </tr>
  </tfoot>
</table>
```

You can use CSS to style tables. Specifically, you can style the various aspects mentioned above.

```css
table,
th,
td {
  border: 1px solid black;
  font-family: Arial, sans-serif;
  text-align: center;
}
```
