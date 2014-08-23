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
```css
a:link {color:blue;} /* select any anchor elements who is hyperlinks and set color to blue */
:link {color:blue;} /* select any link and set color to blue */
:visited {color:gray;} /* select any hyperlinks that have been visited, and set color to gray */
```
