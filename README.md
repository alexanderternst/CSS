# CSS

- [CSS](#css)
- [Flexbox](#flexbox)
  - [Properties for the Parent (flex container)](#properties-for-the-parent-flex-container)
  - [Properties for the Children (flex items)](#properties-for-the-children-flex-items)
  - [Examples](#examples)
  - [Display Types](#display-types)
  - [Old versions of flex](#old-versions-of-flex)
- [Grid](#grid)
  - [Important terminology](#important-terminology)
  - [CSS Grid Properties](#css-grid-properties)
  - [Properties for the Parent (Grid Container)](#properties-for-the-parent-grid-container)
    - [display:](#display)
    - [grid-template-columns, grid-template-rows:](#grid-template-columns-grid-template-rows)
    - [grid-template-area:](#grid-template-area)
  - [Properties for the Children (Grid Items)](#properties-for-the-children-grid-items)
  - [Special Units](#special-units)
  - [Some notes](#some-notes)
  - [Position attribute](#position-attribute)
- [Resources](#resources)

# Flexbox
Horizontal or vertical one dimensional layout.  
Flexbox is a module not a property, so some properties are meant for the container (parent element, known as "flex container") and some are meant to be set on the children (said "flex items").

![Image explaining flexbox](https://css-tricks.com/wp-content/uploads/2018/11/00-basic-terminology.svg)
Items will be laid out following either the main axis (from main-start to main-end) or the cross axis (from cross-start to cross-end).

*Note:* Flexbox layout is most appropriate to the components of an application, and small-scale layouts, while the Grid layout is intended for larger scale layouts.

## Properties for the Parent (flex container)
- display
  - This defines a flex container; inline or block depending on the given value. It enables a flex content fot all its direct children (flex or inline-flex)
- flex-direction
  - This establishes the main-axis, thus defining the direction flex items are places in the flex container (row, row-reverse, column, column-reverse)
- flex-wrap
  - By default, flex items will all try to fit onto one line. You can change that and allow the items to wrap as needed with this property (nowrap, wrap, wrap-reverse)
- flex-flow
  - Shorthand for flex-direction and flex-wrap
- justify-content
  - This defines the alignment along the main axis
- align-items
  - This defines the default behavior for how flex items are laid out along the cross axis on the current line. Think of it as the justify-content version for the cross-axis.
- align-content
  - This aligns a flex container’s lines within when there is extra space in the cross-axis, similar to how justify-content aligns individual items within the main-axis.
- gap, row-gap, column-gap 
  - The gap property explicitly controls the space between flex items. It applies that spacing only between items not on the outer edges.

## Properties for the Children (flex items)
- order
  - By default flex item are laid out in the source order. However the order property controls the order property controls the order in which they appear in the flex container. Items with the same order revert to source order.
- flex-grow
  - This defines the ability for a flex item to grow if necessary. It accepts a unitless value that serves as a proportion. It dictates what amount of the available space inside the flex container the item should take up. (default: 0)
  - If all items have flex-grow set to 1, the remaining space in the container will be distributed equally to all children. If one of the children has a value of 2, that child would take up twice as much of the space either one of the others (or it will try, at least).
  - Very similar to the 1* and 2* in xaml
- flex-shrink
  - This defines the ability for a flex item to shrink if necessary (default: 1)
- flex-basis
  - This defines the default size of an element before the remaining space is distributed. It can be a length (e.g. 20%, 5rem, etc.) or a keyword. The auto keyword means “look at my width or height property” (which was temporarily done by the main-size keyword until deprecated). The content keyword means “size it based on the item’s content” – this keyword isn’t well supported yet, so it’s hard to test and harder to know what its brethren max-content, min-content, and fit-content do.
  - If set to 0, the extra space around content isn’t factored in. If set to auto, the extra space is distributed based on its flex-grow value. See this graphic.
  - ![](https://www.w3.org/TR/css-flexbox-1/images/rel-vs-abs-flex.svg)
- flex
  - This is the shorthand for flex-grow, flex-shrink and flex-basis combined. The second and third parameters (flex-shrink and flex-basis) are optional.
- align-self
  - This allows the default alignment (or the one specified by align-items) to be overridden for individual flex items.

## Examples
See folder

## Display Types
The display CSS property sets whether an element is treated as a block or inline box and the layout used for its children, such as flow layout, grid or flex. Here are some important types.
- block
  - A block-level element always starts on a new line, and the browsers automatically add some space (a margin) before and after the element. A block-level element always takes up the full width available (stretches out to the left and right as far as it can).
- inline-block
  - Compared to display: inline, the major difference is that display: inline-block allows to set a width and height on the element.
  - Also, with display: inline-block, the top and bottom margins/paddings are respected, but with display: inline they are not.
  - Compared to display: block, the major difference is that display: inline-block does not add a line-break after the element, so the element can sit next to other elements.
- none (display nothing)
- grid (will  be covered in different section)

## Old versions of flex
- If you are looking at a blog post (or whatever) about Flexbox and you see display: box; or a property that is box-{*}, you are looking at the old 2009 version of Flexbox.
- If you are looking at a blog post (or whatever) about Flexbox and you see display: flexbox; or the flex() function, you are looking at an awkward tweener phase in 2011.
- If you are looking at a blog post (or whatever) about Flexbox and you see display: flex; and flex-{*} properties, you are looking at the current (as of this writing) specification.

# Grid
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
### display:

Defines the element as a grid container and establishes a new grid formatting context for its contents.
Values: `grid` for a block-level grid and `inline-grid` for an inline-level grid

### grid-template-columns, grid-template-rows:

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

### grid-template-area:

Defines a grid template by referencing the names of the grid areas which are specified with the grid-area property. Repeating the name of a grid area causes the content to span those cells. A period signifies an empty cell. The syntax itself provides a visualization of the structure of the grid.

*Values:*
- `grid-area-name` - the name of a grid area specified with grid-area
- `.` - a period signifies an empty grid cell
- `none` - no grid areas are defined

## Properties for the Children (Grid Items)
- grid-column-start, grid-column-end, grid-row-start, grid-row-end
- grid-column, grid-row
- grid area
- justify-self
- align-self
- place-self

## Special Units
- fr units
- sizing keywords
- sizing functions
- repeat()/minmax()
- masonry
- subgrid

## Some notes
- Something like a navbar should not be in the grid

## Position attribute
- Position attribute
- Column and Row/Table (more bootstrap stuff)

# Resources
- https://css-tricks.com/snippets/css/a-guide-to-flexbox/
- https://developer.mozilla.org/en-US/docs/Web/CSS
- https://flexboxfroggy.com/
- https://mastery.games/flexboxzombies/
- https://getbootstrap.com/docs/4.3/utilities/flex/
- https://nekocalc.com/px-to-rem-converter
- https://codepen.io/samnorton/pen/zBYMJG
- https://developer.mozilla.org/en-US/docs/Web/CSS/display
- https://web.dev/learn/css/box-model/
- https://css-tricks.com/snippets/css/complete-guide-grid/
- https://cssgridgarden.com/