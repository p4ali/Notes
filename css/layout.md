## CSS Box
When any element is rendered, it occupies a rectangular region of visual space. The rectangular are referred to as **CSS boxes**.

For instance, every visible <p> element on a page creates an individula rectangle on the screen inside which the content of 
that paragraph is displayed, or *flows into*. All of a paragraph's content, typically text, also render as CSS boxes.

The <p> elements create what are known as **block-level** boxes, whereas the strings of text within them create **inline-level** boxes.

## Rule for document flow
How CSS boxes flow when they are rendered is by the following rule:
> block-level boxes always flow one on tope of another, like bricks in a vertical stack. 
> inline-level boxes flow one after the other horizontally, literally in the same horizontal line as any neighboring inline elements.
> A user agent's build-in style sheet determines what elements create what kinds of CSS boxes.
