# `<table>`

The table element itself is unusual in how wide it is. It behaves like a block-level element in that if you put one table after another, each will break down onto its own line. But the actual width of the table is only as wide as it needs to be.

If the amount of text in the tables widest row only happens to be `100px` wide, the table will be `100px` wide. If the amount of text (if put on one line) would be wider than the container, it will wrap.

However if text is told to not wrap (i.e. white-space: nowrap;) the table is happy to bust out of the container and go wider. Table cells will also not wrap, so if there are too many to fit, the table will also go wider.

## `<tfoot>`

Along with `<thead>` and `<tbody>` there is `<tfoot>` for wrapping table rows that indicate the footer of the table.

`<tfoot>` is required to be after `<thead>` and before `<tbody>`!

## All Table Related Elements

| Element      | What it is                                                |
| ------------ | --------------------------------------------------------- |
| `<table>`    | The table itself                                          |
| `<caption>`  | The caption for the table. Like a figcaption to a figure. |
| `<thead>`    | The table header                                          |
| `<tbody>`    | The table body                                            |
| `<tfoot>`    | The table footer                                          |
| `<tr>`       | A table row                                               |
| `<th>`       | A table cell that is a header                             |
| `<td>`       | A table cell that is data                                 |
| `<col>`      | A column (a no-content element)                           |
| `<colgroup>` | A group of columns                                        |

## All Table Related Attributes

| Attribute | Element(s) Found On | What it does                                                                                                              |
| --------- | ------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| colspan   | th, td              | extends a cell to be as wide as 2 or more cells                                                                           |
| rowspan   | th, td              | extends a cell to be as tall as 2 or more cells                                                                           |
| span      | col                 | Makes the column apply to more to 2 or more columns                                                                       |
| sortable  | table               | Indicates the table should allow sorting. UPDATE: I’m told this was removed from spec because of lack of implementations. |
| headers   | td                  | space-separated string corresponding to ID’s of the `<th>` elements relevant to the data                                  |
| scope     | th                  | row \| col \| rowgroup \| colgroup (default) – specifies the axis of the header.                                          |

### These properties are either unique to table elements or they behave uniquely on table elements

| CSS Property    | Possible Values                                                             | What it does                                                                          |
| --------------- | --------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| vertical-align  | baseline, sub, super, text-top, text-bottom, middle, top, bottom, %, length | Aligns content vertically within a table cell.                                        |
| white-space     | normal, pre, nowrap, pre-wrap, pre-line                                     | Controls text wrapping within a cell.                                                 |
| border-collapse | collapse, separate                                                          | Determines if table borders collapse into each other or remain separate.              |
| border-spacing  | length                                                                      | Specifies spacing between adjacent table cells when using "separate" border-collapse. |
| width           | length                                                                      | Sets the width of table cells and defines the overall table width.                    |
| border          | length                                                                      | Defines the border width for table elements, including cells.                         |
| table-layout    | auto, fixed                                                                 | Determines how table and cell widths are calculated, affecting rendering behavior.    |

## Making a Table Not a Table

```css
th,
td {
  display: inline;
}
```

Just to be safe, it may be safer to reset all table elements:

```css
table,
thead,
tbody,
tfoot,
tr,
td,
th,
caption {
  display: block;
}
```

## Zebra Striping Tables

```css
tr:nth-child(odd) {
  background-color: #eee;
}
```

## Highlighting a column

A table with four columns in each row would have four `<col>` elements:

```html
<table>
  <col>
  <col>
  <col>
  <col>

  <thead>
     ...

</table>
```

Then you could highlight a particular one, like:

```css
col:nth-child(3) {
  background: yellow;
}
```
