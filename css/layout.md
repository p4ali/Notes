## CSS Box
When any element is rendered, it occupies a rectangular region of visual space. The rectangular are referred to as **CSS boxes**.

For instance, every visible <p> element on a page creates an individula rectangle on the screen inside which the content of 
that paragraph is displayed, or *flows into*. All of a paragraph's content, typically text, also render as CSS boxes.

The <p> elements create what are known as **block-level** boxes, whereas the strings of text within them create **inline-level** boxes.

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

{display: inline;} /* the element generate on or more inlie element boxes*/
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
**position** and **float** also affect how CSS box is rendered.

**position**:
* static
* relative
* absolute
* fixed

**float**:
* left
* right

```css

```


[CSS Reference](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference)
