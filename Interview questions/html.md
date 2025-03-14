## HTML : HyperText Markup Language
is a **language** used to structure and display text and other media on the web, where text can be linked to other content(**hypertext** i.e hyperlink, and its structure is defined by **markup** tags.

## Version
The current version of HTML is still **HTML5**, and it was finalized in **2014**. However, since its finalization, HTML5 has continued to evolve with **minor updates and improvements** to accommodate **new web technologies and features**.

## Element
An element in HTML is a fundamental building block of a webpage. It consists of a **start tag**, **optional content**, and an **end tag**. Each element represents a specific **piece of content** or a **part of the structure** of a webpage.

```html
<p>This is a paragraph.</p>
```
Some elements are self-closing and don't require an end tag, like the `<input>`, `<img>` tag:
```html
<img src="image.jpg" alt="Description of image">
<input type="text/>
```
## Semantic elements vs Non-semantic elements
1. Semantic elements describe both the **structure and meaning** of your content (like `<header>, <footer>, <article>`, etc.).
- it helps with SEO(search engines), accessibility(screen readers for visually challanged), and maintaining clear, understandable code(developers).
    1. **`<header>`**: Contains introductory content or navigation.
    2. **`<footer>`**: Contains footer content, like copyright.
    3. **`<article>`**: Represents a standalone piece of content like blog post/new article.
    4. **`<section>`**: Groups related content into sections.
    5. **`<nav>`**: Contains navigation links.
    6. **`<aside>`**: Represents tangential content (e.g., sidebar).
    7. **`<main>`**: Contains the primary content of a page.
    8. **`<mark>`**: Highlights(bg yellow color) important text.
    9. **`<figure>`**: Contains media content with a caption.
    10. **`<figcaption>`**: Provides a caption for `<figure>`.
2. Non-semantic elements are generic tags that **don’t give any clue** about the content they contain (like `<div>, <span>`).
- useful for certain structural purposes or when no semantic tag fits or not neccessary.


## defer & async aatributes in script tag -accenture hackerrank test
```js
var script = document.createElement('script');
script.src = "https://example.com/script.js";
script.async = true;
document.head.appendChild(script);
```
```html
<script scr="https://example.com/script.js" async></script>
```
| **Attribute** | **Behavior** | **When to Use** |
|---------------|--------------|-----------------|
| **Default**   | Blocks HTML parsing while downloading and executing the script. | When you need immediate execution, and order of script file excecution matters. |
| **`async`**   | Downloads script asynchronously, executes immediately once ready (no blocking). | For independent scripts (e.g., analytics), execution order doesn’t matter. |
| **`defer`**   | Downloads script asynchronously, executes after HTML is fully parsed (order is preserved). | For DOM-dependent scripts or when script execution order is important. |



## how to make div editable? - accenture hackerrack test
```html
<div contenteditable="true">
  This text can be edited by the user.
</div>
```
