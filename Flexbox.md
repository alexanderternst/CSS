# Flexbox

- [Flexbox](#flexbox)
  - [Properties for the Parent (flex container)](#properties-for-the-parent-flex-container)
  - [Properties for the Children (flex items)](#properties-for-the-children-flex-items)
  - [Examples](#examples)
  - [Display Types](#display-types)
  - [Old versions of flex](#old-versions-of-flex)

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
  - Very similar to the "1*" and "2*" in xaml
- flex-shrink
  - This defines the ability for a flex item to shrink if necessary (default: 1)
- flex-basis
  - This defines the default size of an element before the remaining space is distributed. It can be a length (e.g. 20%, 5rem, etc.) or a keyword. The auto keyword means “look at my width or height property” (which was temporarily done by the main-size keyword until deprecated). The content keyword means “size it based on the item’s content” – this keyword isn’t well supported yet, so it’s hard to test and harder to know what its brethren max-content, min-content, and fit-content do.
  - If set to 0, the extra space around content isn’t factored in. If set to auto, the extra space is distributed based on its flex-grow value. See this graphic.
  - ![flex-basis](https://www.w3.org/TR/css-flexbox-1/images/rel-vs-abs-flex.svg)
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
