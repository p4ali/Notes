## CSS selector
### Chained selector
Selector literally chained without any space or coma.
```css
h1  /* Simple selector: select all h1 */
div.vcard  /* 2 Chanined selector: select all div with class vcard */
div.vcard[title]  /* 3 Chained selector: select all div with class vcard and an attribute title */
```

### Selector group
Selector literally sit together separated with comma
```css
div, .vcard /* A selector group with two selectors: select all divs and all elements with vcard class */
```

### Simple selectors
#### Type selector
```css
div /* select elements div */
* { border:1px solid #400080;} /* Universal type selector: (*)select any element and set border 1 pixel*/
```

#### Attribute selector
```css
[foo] /* select elements who has attribute named foo */
[foo=bar] /* select elements who has attribute named foo, and value equals bar */
[foo~=bar] /* select elements who has attribute named foo, and value contains world bar, not abarrrrr */
[foo|=bar] /* select elements whos foo attribute contains a hyphen-separated list of words and the first word is exactly bar, for example, bar-1, bar-2-3-4-5, this is used to match  lang="zh-cn", lang="zh-hk" */

/* The following behaves just like regex */
[title^=bass] /* select elements with title attribute whose value begins with bass */
[title$=bass] /* select elements with title attribute whose value end with bass */
[title*=bass] /* select elements with title attribute whose value contains bass */
```



#### ID and class selector
```css
p#conductor /* select the p element whose id equals conductor */
#conductor /* select the element whose id equals to conductor, but since id is uinique amond all elements in a document, this is actually select the same p as above, iff there is a p with id conductor */
.orchestra /* select elements whose class attribute equals to orchestra */
```

#### Psuedo-classes
Pseduo-classes are a way to target elements that have a particular characteristic, which may not necessarily the element's attribute and value pairs. YOu use a colon **(:)** to delimit the beginning of a pseudo-class selector.

##### Hyperlinks
```css
/* syntax */
selector:pseudo-class { property: value; }

a:link {color:blue;} /* select any anchor elements who is hyperlinks and set color to blue */
:link {color:blue;} /* select any link and set color to blue */
:visited {color:gray;} /* select any hyperlinks that have been visited, and set color to gray */
```

##### User intractions: Dynamic pseudo-classes
These apply to elements the user has acted upon in some way. In most cases, it turned to be a hyperlink.
```css
:active /* select element that are currently active, e.g., when a user clicks a hyperlink and has not yet released the mouse button or when user clicks a button on a form. Another example is by pressing a return or enter key on keyboard while the element has keyboard focus */
:focus /* select element which has focus. It make sense to group :hover and :focus together */
:hover /* select element which is hovering on */
```

##### :lang 
```css
:lang(zh) /* select element who has lang attribute with zh*/
[lang|=zh] /* same as above*/
```

##### Document fragment :target
```css
#conductor:target { font: 1.2em Helvetica, Sans-serif; } /* select conductor element when accessed via a link with a fragment identifier */
```

##### First or last element
```css
div h2:first-child /* first h2 element within any div */
body > :last-child /* very last child of body element */
:first-child /* match all elements that are 1st children of their parents */
tr:last-child td:first-child /* match all the 1st table cell in the last roe of any tables */
```

#### [Pseudo-elements](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements)
Pseudo-elements select fictional elements that don't really exist in the markup. You can think of them as abstractions to target smaller parts of a larger element that would be impossible to syle in any other way.
```css
/* syntax */
selector:pseudo-element { property: value; }

p::first-letter {font-size: 1.5em; } /* emphasize set first letter include the number, and punctuation of the paragraph */
q::first-line /* select the first line on the fly, when browser resize. Only meaningful in a block-container box */
p::before /* create a pseudo-element that is the first child of the element matched. It is often used to add cosmetic content to an element by using the content property. This element is inline by default. */
p::after /* create a pseudo-element that is the last child of the selected element */
p::selection /* apply to the portion of a document that has been hilighted */
```

### Combinator
Simple selectors can be chained to create sequences that increase a selector's specificity and fildter the selector's subjects to a more precise set of elements. Combinators can be used to chain sequences of simple selectors together with a similar effect. Combinator combine sequences of selectors, and the subjects of the selectors on the righ-hand side of the combinator have a particular relationship to the subjects of the selectors on the left-hand side.

#### Context
Context is position and relationship within the DOM tree.

#### Descendant combinator
```css
/* syntax */
selector1 selector2 { style properties } /* selector2 selected elements must be descendants of selector1 */

div span {background-color: DodgerBlue; } /* set background color of those spans who is descendant of div to DodgerBlue */
```
#### Child combinator
```css
div > p {color: #777} /* select p who is the immediate child of the div, and set color to #777 */
```

#### Adjacent sibling combinator
Selects the subject on its right if that element is the sibling immediately following the subject on its left.
```css
h1 + p /* select only the first paragraph */
h1 ~ p /* select all of the paragraphs who are siblings of h1 */
```
