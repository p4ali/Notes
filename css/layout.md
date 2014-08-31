## CSS Box
When any element is rendered, it occupies a rectangular region of visual space. The rectangular are referred to as **CSS boxes**.

For instance, every visible &lt;p&gt; element on a page creates an individula rectangle on the screen inside which the content of 
that paragraph is displayed, or *flows into*. All of a paragraph's content, typically text, also render as CSS boxes.

The &lt;p&gt; elements create what are known as **block-level** boxes, whereas the strings of text within them create **inline-level** boxes.

### 3 Types of elements

* **block-level** elements start on a new line, e.g., h1, p, ul, li
* **inline-level** elements flow in between surrounding text, e.g., img, b, i
* **containing or parent** elemnts are block-level elements who have block-level children inside.

### Rule for document flow
How CSS boxes flow when they are rendered is by the following rule:
> **block-level** boxes always flow one on tope of another, like bricks in a vertical stack. 
> **inline-level** boxes flow one after the other horizontally, literally in the same horizontal line as any neighboring inline elements.
> A user agent's build-in style sheet determines what elements create what kinds of CSS boxes.

CSS boxes interact with one another in particular ways. *Inline boxes* are always rendered inside a *block-levelbox*, known as their **containing block**.

If a CSS box's containing block is sized at unchanging dimensions and the box is too large to fit within it, then it will **overflow** the container. However, the overflow doesn't typically affect the visual positioning of any other CSS boxes inside of different containing blocks, such as text in the next paragraph.

### Properties affect CSS box
The **display** property determines what kind of CSS box it will create.
```css
/* Syntax */
{display: none;} /* turns off the display of an element. Which means document will not render the element and its desendants */

{display: inline;} /* the element generate one or more inline element boxes*/
{display: block;} /* the element generates a block element box */
{display: list-item;} /* the element generates a block box for the content and a separate list-litem inline box. */
{display: inline-block;} /* the element generate a block element box that will be flowed with surronding content as if it were a single inline box*/
{display: inline-table;} /* The inline-table value does not have a direct mapping in HTML. It behaves like a <table> HTML element, but as an inline box, rather than a block-level box. Inside the table box is a block-level context*/
{display: table;} /* behaves linke the <table> HTML element. It defines a block-level box. */
{display: table-cell;} /* behaves like a <td> HTML element */
{display: table-column;} /* behave like the <col> HTML elements */
{display: table-column-group;} /* behave like the <colgroup> HTML elements */
{display: table-footer-group;} /* behave like <tfoot> HTML elements */
{display: table-header-group;} /* behave like <thead> HTML elements */
{display: table-row;} /* behave like <tr> HTML element */
{display: table-row-group;} /* behave like <tbody> HTML element */
{display: flex;} /* The element behaves like a block element that lays out its content according to the flexbox model */
{display: inline-flex;} /* The element behaves like an inline element and lays out its content according to the flexbox model. */
{display: grid;} /* The element behaves like a block element and lays out its content according to the grid model. */
{display: inline-grid;} /* The element behaves like an element and lays out its content according to the grid modeol **/
{display: run-in;} /* If the run-in box contains a block box, same as block; If a block box follows the run-in box, the run-in box becomes the first inlinie box of the block box.; If an inline box follows, teh run-in box becomes a block box. */

display: inherit

/* Here is the default CSS rules that web browse use to make <p> elements block-level and <strong> elements inline-level might look like: */
p { display: block; }
strong { display: inline; }
li { display: list-item; }
table { display: table; }
row { display: table-row; }
col { display: table-column; }
```

## Positioning schemes
* **{position: static;} normal flow** - every block-level element on a new line, sits on top of the next one
* **{position: relative;} relative positioning** - position element by shifting it to rop, righ, botton or thers
* **{position: absolute;} absolute positioning** - position element relate to container
* **{position: fixed;} fixed positioning** - a type of absolute positioning. It positions the element relate to browser window
* **{float: left|right;} floating** - floating elements allows you to take the element in normal flow and place it to the far left or right of a containing box.
* **{clear: left|right|both|none;} clear** - no element (within the containing element) should touch the left or right hand sides of a box.
* **{z-index:10;} z-index** - When you move any element from normal flow, boxed can overlap. Use **z-index** property to control which box appreas on top.
* **{overflow: auto; width: 100%;}** - to make parents of floated elements auto-resize to contain the children.

```css

```


[CSS Reference](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference)
