# Grid

- [Grid](#grid)
  - [Important terminology](#important-terminology)
  - [CSS Grid Properties](#css-grid-properties)
  - [Properties for the Parent (Grid Container)](#properties-for-the-parent-grid-container)
    - [display](#display)
    - [grid-template-columns, grid-template-rows](#grid-template-columns-grid-template-rows)
    - [grid-template-area](#grid-template-area)
    - [column-gap row-gap grid-column-gap grid-row-gap](#column-gap-row-gap-grid-column-gap-grid-row-gap)
    - [justify-items](#justify-items)
    - [align-items](#align-items)
    - [place-items](#place-items)
    - [justify-content](#justify-content)
    - [align-content](#align-content)
    - [place-content](#place-content)
    - [grid](#grid-1)
  - [Properties for the Children (Grid Items)](#properties-for-the-children-grid-items)
    - [grid-column-start, grid-column-end, grid-row-start, grid-row-end](#grid-column-start-grid-column-end-grid-row-start-grid-row-end)
    - [grid-column, grid-row](#grid-column-grid-row)
    - [grid-area](#grid-area)
    - [justify-self](#justify-self)
    - [align-self](#align-self)
    - [place-self](#place-self)
  - [Fluid columns snippet](#fluid-columns-snippet)
  - [Special Units](#special-units)
  - [Some notes](#some-notes)

CSS Grid Layout (aka “Grid” or “CSS Grid”), is a two-dimensional grid-based layout system that, compared to any web layout system of the past, completely changes the way we design user interfaces. CSS has always been used to layout our web pages, but it’s never done a very good job of it. First, we used tables, then floats, positioning and inline-block, but all of these methods were essentially hacks and left out a lot of important functionality (vertical centering, for instance). Flexbox is also a very great layout tool, but its one-directional flow has different use cases — and they actually work together quite well! Grid is the very first CSS module created specifically to solve the layout problems we’ve all been hacking our way around for as long as we’ve been making websites.

To get started you have to define a container element as a grid with display: grid, set the column and row sizes with grid-template-columns and grid-template-rows, and then place its child elements into the grid with grid-column and grid-row.

## Important terminology

- grid container
  - The element on which `display: grid` is applied. Its the direct parent of all the grid items
- grid item
  - The children  (i.e. direct descendants) of the grid container.
- grid line
  - The dividing lines that make up the structure of the grid. They can be either vertical (“column grid lines”) or horizontal (“row grid lines”).
  - Here the yellow line is an example of a column grid line.
  - ![in yellow: grid line](https://css-tricks.com/wp-content/uploads/2018/11/terms-grid-line.svg)
- grid cell
  - The space between two adjacent row and two adjacent column grid lines. It’s a single “unit” of the grid.
  - Here’s the grid cell between row grid lines 1 and 2, and column grid lines 2 and 3.
  - ![in yellow: grid cell](https://css-tricks.com/wp-content/uploads/2018/11/terms-grid-cell.svg)
- grid track
  - The space between two adjacent grid lines. You can think of them as the columns or rows of the grid.
  - Heres the grid track between the second and third-row grid lines
  - ![grid track](https://css-tricks.com/wp-content/uploads/2021/08/terms-grid-track.svg)
- grid area
  - The total space surrounded by four grid lines. A grid area may be composed of any number of grid cells.
  - Here’s the grid area between row grid lines 1 and 3, and column grid lines 1 and 3.
  - ![grid area](https://css-tricks.com/wp-content/uploads/2018/11/terms-grid-area.svg)

## CSS Grid Properties

There are to many properties to cover properly so I will only cover the completly essentials here, go to the [CSS Tricks article](https://css-tricks.com/snippets/css/complete-guide-grid/#aa-css-grid-properties) for more information.

## Properties for the Parent (Grid Container)

### display

Defines the element as a grid container and establishes a new grid formatting context for its contents.
Values: `grid` for a block-level grid and `inline-grid` for an inline-level grid

### grid-template-columns, grid-template-rows

Defines the columns and rows of the grid with a space-separated list of values. The values represent the track size, and the space between them represents the grid line.

*Values:*

- `track-size` – can be a length, a percentage, or a fraction of the free space in the grid using the fr unit (more on this unit over at DigitalOcean)
- `line-name` – an arbitrary name of your choosing

*Example:*

```css
.container {
  grid-template-columns: ...  ...;
  /* e.g. 
      1fr 1fr
      minmax(10px, 1fr) 3fr
      repeat(5, 1fr)
      50px auto 100px 1fr
  */
  grid-template-rows: ... ...;
  /* e.g. 
      min-content 1fr min-content
      100px 1fr max-content
  */
}
```

Grid lines are automatically assigned positive numbers from these assignments (-1 being an alternate for the very last row).  
![css grid without explicit naming](https://css-tricks.com/wp-content/uploads/2018/11/template-columns-rows-01.svg)

But you can choose to explicitly name the lines. Note the bracket syntax for the line names:  

```css
  .container {
    grid-template-columns: [first] 40px [line2] 50px [line3] auto [col4-start] 50px [five] 40px [end];
    grid-template-rows: [row1-start] 25% [row1-end] 100px [third-line] auto [last-line];
  }
```

![css grid with explicit naming](https://css-tricks.com/wp-content/uploads/2018/11/template-column-rows-02.svg)

- If your definition contains repeating parts, you can use the repeat() notation to streamline things.
- If multiple lines share the same name, they can be referenced by their line name and count.
- The fr unit allows you to set the size of a track as a fraction of the free space of the grid container. For example, this will set each item to one third the width of the grid container.
  - The free space is calculated after any non-flexible items. In this example the total amount of free space available to the fr units doesn’t include the 50px.

```css
/* repeat syntax */
.container {
  grid-template-columns: repeat(3, 20px [col-start]);
}

/* multiple lines with the same name */
.item {
  grid-column-start: col-start 2;
}

/* fr unit */
.container {
  grid-template-columns: 1fr 1fr 1fr;
}

/* fr unit is distributed after any non-flexible items */
.container {
  grid-template-columns: 1fr 50px 1fr 1fr;
}
```

### grid-template-area

Defines a grid template by referencing the names of the grid areas which are specified with the grid-area property. Repeating the name of a grid area causes the content to span those cells. A period signifies an empty cell. The syntax itself provides a visualization of the structure of the grid.

*Values:*

- `grid-area-name` - the name of a grid area specified with grid-area
- `.` - a period signifies an empty grid cell
- `none` - no grid areas are defined

*Example:*

```css
.item-a {
  grid-area: header;
}
.item-b {
  grid-area: main;
}
.item-c {
  grid-area: sidebar;
}
.item-d {
  grid-area: footer;
}

.container {
  display: grid;
  grid-template-columns: 50px 50px 50px 50px;
  grid-template-rows: auto;
  grid-template-areas: 
    "header header header header"
    "main main . sidebar"
    "footer footer footer footer";
}
```

That’ll create a grid that’s four columns wide by three rows tall. The entire top row will be composed of the header area. The middle row will be composed of two main areas, one empty cell, and one sidebar area. The last row is all footer.
![graphic of grid-template-area](https://css-tricks.com/wp-content/uploads/2018/11/dddgrid-template-areas.svg)

### column-gap row-gap grid-column-gap grid-row-gap

Specifies the size of the grid lines. You can think of it like setting the width of the gutters between the columns/rows.

*Values:*

- `<line-size>` - a length value

```css
.container {
  /* standard */
  column-gap: <line-size>;
  row-gap: <line-size>;

  /* old */
  grid-column-gap: <line-size>;
  grid-row-gap: <line-size>;
}
```

*Example:*

```css
.container {
  grid-template-columns: 100px 50px 100px;
  grid-template-rows: 80px auto 80px; 
  column-gap: 10px;
  row-gap: 15px;
}
```

![column and row gap in css grid](https://css-tricks.com/wp-content/uploads/2018/11/dddgrid-gap.svg)
The gutters are only created between the columns/rows, not on the outer edges.

### justify-items

Aligns grid items along the inline (row) axis (as opposed to align-items which aligns along the block (column) axis). This value applies to all grid items inside the container.

*Values:*

- `start` - aligns items to be flush with the start edge of their cell
- `end` - aligns items to be flush with the end edge of their cell
- `center` - aligns items in the center of their cell
- `stretch` - fills the whole width of the cell (this is the default)

```css
.container {
  justify-items: start | end | center | stretch;
}
```

*Examples:*

```css
.container {
  justify-items: start;
}
```

![justify-items css grid](https://css-tricks.com/wp-content/uploads/2018/11/justify-items-start.svg)

```css
.container {
  justify-items: end;
}
```

![justify-items css grid](https://css-tricks.com/wp-content/uploads/2018/11/justify-items-end.svg)

```css
.container {
  justify-items: center;
}
```

![justify-items css grid](https://css-tricks.com/wp-content/uploads/2018/11/justify-items-center.svg)

```css
.container {
  justify-items: stretch;
}
```

![justify-items css grid](https://css-tricks.com/wp-content/uploads/2018/11/justify-items-stretch.svg)
This behavior can also be set on individual grid items via the justify-self property.

### align-items

Aligns grid items along the block (column) axis (as opposed to justify-items which aligns along the inline (row) axis). This value applies to all grid items inside the container.

*Values:*

- `stretch` - fills the whole height of the cell (this is the default)
- `start` - aligns items to be flush with the start edge of their cell
- `end` - aligns items to be flush with the end edge of their cell
- `center` - aligns items in the center of their cell
- `baseline` - align items along text baseline. There are modifiers to baseline — first baseline and last baseline which will use the baseline from the first or last line in the case of multi-line text.

```css
.container {
  align-items: start | end | center | stretch;
}
```

*Examples:*

```css
.container {
  align-items: start;
}
```

![align-items css grid](https://css-tricks.com/wp-content/uploads/2018/11/align-items-start.svg)

```css
.container {
  align-items: end;
}
```

![align-items css grid](https://css-tricks.com/wp-content/uploads/2018/11/align-items-end.svg)

```css
.container {
  align-items: center;
}
```

![align-items css grid](https://css-tricks.com/wp-content/uploads/2018/11/align-items-center.svg)

```css
.container {
  align-items: stretch;
}
```

![align-items css grid](https://css-tricks.com/wp-content/uploads/2018/11/align-items-stretch.svg)
This behavior can also be set on individual grid items via the align-self property.

### place-items

place-items sets both the align-items and justify-items properties in a single declaration.

*Values:*

- `<align-items> / <justify-items>` – The first value sets align-items, the second value justify-items. If the second value is omitted, the first value is assigned to both properties.

This can be very useful for super quick multi-directional centering:

```css
.center {
  display: grid;
  place-items: center;
}
```

Do the same things as above just for the entire grid if there is free space in parent element of grid.

### justify-content

Sometimes the total size of your grid might be less than the size of its grid container. This could happen if all of your grid items are sized with non-flexible units like px. In this case you can set the alignment of the grid within the grid container. This property aligns the grid along the inline (row) axis (as opposed to align-content which aligns the grid along the block (column) axis).

- `start` – aligns the grid to be flush with the start edge of the grid container
- `end` – aligns the grid to be flush with the end edge of the grid container
- `center` – aligns the grid in the center of the grid container
- `stretch` – resizes the grid items to allow the grid to fill the full width of the grid container
- `space-around` – places an even amount of space between each grid item, with half-sized spaces on the far ends
- `space-between` – places an even amount of space between each grid item, with no space at the far ends
- `space-evenly` – places an even amount of space between each grid item, including the far ends

```css
.container {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;    
}
```

*Examples (with images):*

```css
.container {
  justify-content: space-around;    
}
```

![align-content css grid](https://css-tricks.com/wp-content/uploads/2018/11/justify-content-space-around.svg)

```css
.container {
  justify-content: space-between;    
}
```

![align-content css grid](https://css-tricks.com/wp-content/uploads/2018/11/justify-content-space-between.svg)
No further examples available here because all the other properties follow the same concept as other styles which were already explained. See the full [CSS Tricks](https://css-tricks.com/snippets/css/complete-guide-grid/#aa-justify-content) article for all examples.

### align-content

Sometimes the total size of your grid might be less than the size of its grid container. This could happen if all of your grid items are sized with non-flexible units like px. In this case you can set the alignment of the grid within the grid container. This property aligns the grid along the block (column) axis (as opposed to justify-content which aligns the grid along the inline (row) axis).

- `start` – aligns the grid to be flush with the start edge of the grid container
- `end` – aligns the grid to be flush with the end edge of the grid container
- `center` – aligns the grid in the center of the grid container
- `stretch` – resizes the grid items to allow the grid to fill the full height of the grid container
- `space-around` – places an even amount of space between each grid item, with half-sized spaces on the far ends
- `space-between` – places an even amount of space between each grid item, with no space at the far ends
- `space-evenly` – places an even amount of space between each grid item, including the far ends

```css
.container {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;    
}
```

*Examples (with images):*

```css
.container {
  align-content: space-around;    
}
```

![align-content css grid](https://css-tricks.com/wp-content/uploads/2018/11/align-content-space-around.svg)

```css
.container {
  align-content: space-between;    
}
```

![align-content css grid](https://css-tricks.com/wp-content/uploads/2018/11/align-content-space-between.svg)
No further examples available here because all the other properties follow the same concept as other styles which were already explained. See the full [CSS Tricks](https://css-tricks.com/snippets/css/complete-guide-grid/#aa-align-content) article for all examples.

### place-content

place-content sets both the align-content and justify-content properties in a single declaration.

*Values:*

- `<align-content> / <justify-content>` – The first value sets align-content, the second value justify-content. If the second value is omitted, the first value is assigned to both properties.

### grid

A shorthand for setting all of the following properties in a single declaration: grid-template-rows, grid-template-columns, grid-template-areas, grid-auto-rows, grid-auto-columns, and grid-auto-flow (Note: You can only specify the explicit or the implicit grid properties in a single grid declaration).

## Properties for the Children (Grid Items)

### grid-column-start, grid-column-end, grid-row-start, grid-row-end

Determines a grid item’s location within the grid by referring to specific grid lines. grid-column-start/grid-row-start is the line where the item begins, and grid-column-end/grid-row-end is the line where the item ends.

*Values:*

- `line` - can be a number to refer to a numbered grid line, or a name to refer to a named grid line
- `span <number>` - the item will span across the provided number of grid tracks
- `span <name>` - the item will span across until it hits the next line with the provided name
- `auto` -  indicates auto-placement, an automatic span, or a default span of one

```css
.item {
  grid-column-start: <number> | <name> | span <number> | span <name> | auto;
  grid-column-end: <number> | <name> | span <number> | span <name> | auto;
  grid-row-start: <number> | <name> | span <number> | span <name> | auto;
  grid-row-end: <number> | <name> | span <number> | span <name> | auto;
}
```

*Examples (with images):*

```css
.item-a {
  grid-column-start: 2;
  grid-column-end: five;
  grid-row-start: row1-start;
  grid-row-end: 3;
}
```

![grid item example](https://css-tricks.com/wp-content/uploads/2018/11/grid-column-row-start-end-01.svg)

```css
.item-b {
  grid-column-start: 1;
  grid-column-end: span col4-start;
  grid-row-start: 2;
  grid-row-end: span 2;
}
```

![grid item example](https://css-tricks.com/wp-content/uploads/2018/11/grid-column-row-start-end-02.svg)

- If no grid-column-end/grid-row-end is declared, the item will span 1 track by default.
- Items can overlap each other. You can use z-index to control their stacking order.

### grid-column, grid-row

Shorthand for grid-column-start + grid-column-end, and grid-row-start + grid-row-end, respectively.

*Values:*

- `<start-line>/<end-line>` - each one accepts all the same values as the longhand version, including span

```css
.item {
  grid-column: <start-line> / <end-line> | <start-line> / span <value>;
  grid-row: <start-line> / <end-line> | <start-line> / span <value>;
}
```

*Example (with image):*

```css
.item-c {
  grid-column: 3 / span 2;
  grid-row: third-line / 4;
}
```

![example of grid-column/grid-row shorthand](https://css-tricks.com/wp-content/uploads/2018/11/grid-column-row.svg)

- If no end line value is declared, the item will span 1 track by default.

### grid-area

Gives an item a name so that it can be referenced by a template created with the grid-template-areas property. Alternatively, this property can be used as an even shorter shorthand for grid-row-start + grid-column-start + grid-row-end + grid-column-end.

*Values:*

- `<name>` - a name of your choosing
- `<row-start> / <column-start> / <row-start> / <column-end>` - can be numbers or named lines

```css
.item {
  grid-area: <name> | <row-start> / <column-start> / <row-end> / <column-end>;
}
```

*Examples:*  
As a way to assign a name to the item:

```css
.item-d {
  grid-area: header;
}
```

As the short-shorthand for grid-row-start + grid-column-start + grid-row-end + grid-column-end:

```css
.item-d {
  grid-area: 1 / col4-start / last-line / 6;
}
```

![example of grid-area](https://css-tricks.com/wp-content/uploads/2018/11/grid-area.svg)

### justify-self

Aligns a grid item inside a cell along the inline (row) axis (as opposed to align-self which aligns along the block (column) axis). This value applies to a grid item inside a single cell.

*Values:*

- `start` - aligns the grid item to be flush with the start edge of the cell
- `end` - aligns the grid item to be flush with the end edge of the cell
- `center` - aligns the grid item in the center of the cell
- `stretch` - fills the whole width of the cell (this is the default)

```css
.item {
  justify-self: start | end | center | stretch;
}
```

*Examples (with images):*

```css
.item-a {
  justify-self: start;
}
```

![justify-self grid item](https://css-tricks.com/wp-content/uploads/2018/11/justify-self-start.svg)

```css
.item-a {
  justify-self: end;
}
```

![justify-self grid item](https://css-tricks.com/wp-content/uploads/2018/11/justify-self-end.svg)

```css
.item-a {
  justify-self: center;
}
```

![justify-self grid item](https://css-tricks.com/wp-content/uploads/2018/11/justify-self-center.svg)

```css
.item-a {
  justify-self: stretch;
}
```

![justify-self grid item](https://css-tricks.com/wp-content/uploads/2018/11/justify-self-stretch.svg)
To set alignment for all the items in a grid, this behavior can also be set on the grid container via the justify-items property.

### align-self

Aligns a grid item inside a cell along the block (column) axis (as opposed to justify-self which aligns along the inline (row) axis). This value applies to the content inside a single grid item.

*Values:*

- `start` - aligns the grid item to be flush with the start edge of the cell
- `end` - aligns the grid item to be flush with the end edge of the cell
- `center` - aligns the grid item in the center of the cell
- `stretch` - fills the whole height of the cell (this is the default)

```css
.item {
  align-self: start | end | center | stretch;
}
```

*Examples (with images):*

```css
.item-a {
  align-self: start;
}
```

![align-self grid item](https://css-tricks.com/wp-content/uploads/2018/11/align-self-start.svg)

```css
.item-a {
  align-self: end;
}
```

![align-self grid item](https://css-tricks.com/wp-content/uploads/2018/11/align-self-end.svg)

```css
.item-a {
  align-self: center;
}
```

![align-self grid item](https://css-tricks.com/wp-content/uploads/2018/11/align-self-center.svg)

```css
.item-a {
  align-self: stretch;
}
```

![align-self grid item](https://css-tricks.com/wp-content/uploads/2018/11/align-self-stretch.svg)
To align all the items in a grid, this behavior can also be set on the grid container via the align-items property.

### place-self

`place-self` sets both the `align-self` and `justify-self` properties in a single declaration (shorthand for align-self and justify-self).

*Values:*

- `auto` - The "default" alignment for the layout mode
- `<align-self>/<justify-self>` - The first value sets align-self, the second value justify-self. If the second value is omitted, the first value is assigned to both properties

*Examples (with images):*

```css
.item-a {
  place-self: center;
}
```

![place-self css grid](https://css-tricks.com/wp-content/uploads/2018/11/place-self-center.svg)

```css
.item-a {
  place-self: center stretch;
}
```

![place-self css grid](https://css-tricks.com/wp-content/uploads/2018/11/place-self-center-stretch.svg)
All major browsers except Edge support the place-self shorthand property.

## Fluid columns snippet

Fluid width columns that break into more or less columns as space is available, with no media queries!
See [example](/examples_grid/fluidcolumn.html) for an example.

## Special Units

- fr units
- sizing keywords
- sizing functions
- repeat()/minmax()
- masonry
- subgrid

## Some notes

- Something like a navbar should not be in the grid, but seperate because Grid is for layout of content on the website
