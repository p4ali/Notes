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
