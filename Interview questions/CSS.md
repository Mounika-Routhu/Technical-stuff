## CSS - Cascading style sheets

## Selectors
1. selectors are used to **target HTML elements** to **apply styles******. 
2. These selectors are categorized into several types based on how they target elements.

| **Selector Type**               | **Description**                                                     | **Example**                                   |
| ------------------------------- | ------------------------------------------------------------------- | --------------------------------------------- |
| **1. Universal Selector**       | Targets **all** elements.                                           | `* { margin: 0; }`                            |
| **2. Type Selector**            | Targets elements by their **HTML tag name**.                        | `p { font-size: 16px; }`                      |
| **3. Class Selector**           | Targets elements with a **specific class attribute**.               | `.btn { background: blue; }`                  |
| **4. ID Selector**              | Targets an element with a **specific ID** (unique per page).        | `#header { color: black; }`                   |
| **5. Group Selector**           | Combines multiple selectors to apply the **same style**.            | `h1, h2, .title { font-family: sans-serif; }` |
| **6. Descendant Selector**      | Targets elements that are **inside another element**, at any depth. | `div p { color: gray; }`                      |
| **7. Child Selector (`>`)**     | Targets elements that are **direct children** of a parent.          | `ul > li { list-style: none; }`               |
| **8. Adjacent Sibling (`+`)**   | Targets the **immediate next sibling** of an element.               | `h1 + p { margin-top: 0; }`                   |
| **9. General Sibling (`~`)**    | Targets **all siblings** after the first element.                   | `h1 ~ p { color: red; }`                      |
| **10. Attribute Selector**      | Targets elements with a **specific attribute or attribute value**.  | `input[type="text"] { border: 1px solid; }`   |

## pseudo class selector
1. A pseudo-class selector targets an element based on its **state or position** in the DOM
2. It starts with single colons :, followed by the pseudo-class name.
   
```CSS
selector:pseudo-class {
  /* styles */
}
```
| Pseudo-Class    | Description                             | Example                                |
| --------------- | --------------------------------------- | -------------------------------------- |
| `:hover`        | When user hovers over an element        | `button:hover { background: red; }`    |
| `:active`       | When element is being clicked           | `a:active { color: green; }`           |
| `:focus`        | When element is focused (like an input) | `input:focus { border: 2px solid; }`   |
| `:first-child`  | First child of a parent                 | `li:first-child { color: blue; }`      |
| `:last-child`   | Last child of a parent                  | `li:last-child { font-weight: bold; }` |
| `:nth-child(n)` | Selects the nth child                   | `li:nth-child(2) { color: purple; }`   |
| `:checked`      | Checked radio or checkbox               | `input:checked { background: green; }` |
| `:disabled`     | Disabled input fields                   | `input:disabled { opacity: 0.5; }`     |
| `:visited`      | Visited link                            | `a:visited { color: purple; }`         |

## pseudo element selector
1. A pseudo-element selector allows you to style **specific parts** of an element or **insert content** without adding extra HTML.
2. It starts with two colons ::, followed by the pseudo-element name.
   
```CSS
selector::pseudo-element {
  /* styles */
}
```
| Pseudo-Element   | What it does                              | Example                                |
| ---------------- | ----------------------------------------- | -------------------------------------- |
| `::before`       | Inserts content **before** the element    | `p::before { content: "👉 "; }`        |
| `::after`        | Inserts content **after** the element     | `p::after { content: " ✔️"; }`         |
| `::first-letter` | Styles the **first letter** of an element | `p::first-letter { font-size: 200%; }` |
| `::first-line`   | Styles the **first line** of a paragraph  | `p::first-line { color: red; }`        |
| `::placeholder`  | Styles placeholder text inside inputs     | `input::placeholder { color: gray; }`  |
| `::selection`    | Styles the text **highlighted** by user   | `::selection { background: yellow; }`  |

## Box Modal
1. The Box Model is the core concept that describes how every HTML element is treated as a **rectangular box made up of four parts**:
   1. Content: The actual content: text, image, etc. Controlled by: width, height
      ```CSS
      width: 200px;
      height: 100px;
      ```
   2. Padding: Space between content and border. Increases space inside the box. Background color applies here
      ```CSS
      padding: 20px;
      ```
   3. Border: Line that wraps around padding + content. You can change color, width, style
      ```CSS
      border: 2px solid black;
      ```
   4. Margin: Space outside the border (gap from other elements). Fully transparent, does not get background color
      ```CS
      margin: 10px;
      ```
2. Using box modal, browsers calculate element size and spacing. It includes content, padding, border, and margin — allowing precise layout control
3. `Box-Sizing` is CSS property to calculate size of an element.
4. By default `Box-Sizing: Content-Box`, where element width only includes content; padding and border are added outside.
5. Total Width of element (Content-Box): width + padding (left & right) + border (left & right)
   - eg: If you set element width: 200px & padding:10px & border 2px - total width of element will be 200 + 10x2(padding left & right) + 2x2(border left & right) = 200 + 20 + 4 => 224px
6. If we change `Box-Sizing: Border-Box`, element width includes content, padding, and border
7. Total Width of element (Border-Box): Exactly equals the width you set, padding and border are inside it
   - eg: If you set element width: 200px & padding:10px & border 2px, then Total width = 200px
   - The **actual content area shrinks** to fit within the 200px after subtracting padding and border.
8. Why it's good to have Border-Box is, this makes **sizing more predictable** and **prevents unexpected layout issues**.
9. **Margin is not included in width**. Margin adds space outside the element, affecting layout but not the box’s actual size.
   - final occupied space/ visible space on the page:
     ```sql
      = margin-left + width + margin-right
      = 20px + 200px + 20px = 240px
     ```
**Outline** - not part of box modal
1. Drawn outside the border. Does not affect layout or box size
2. Used for **visible focus indication** - Commonly seen when you **Tab** through form fields or buttons
```css
outline: 2px dashed red;
```

## Why mobile first approach is considered good?
1. **More people use mobile devices** to browse the internet than desktops. - **covering majority of users first**
2. Google focuses on mobile views of websites for ranking, so good for **SEO** => help your site **show up higher in search results**.
3. Mobile-first keeps the design clean and fast from the start, while desktop-first often leads to **bloated mobile versions**.
4. Desktop designs often include extra visuals, animations, and features. When you try to shrink it down for mobile, **it’s hard to remove or adapt things cleanly**. Result: **Overloaded mobile site that’s slow or cluttered**.
5. Mobile-first starts light. You begin with the **core content and layout**. Then you progressively **enhance for larger screens**. Result: **Fast, focused, and scalable — works well on all devices.**
   
## Standard media queries
Media Queries are a feature in CSS that let you apply styles based on the device's screen size, resolution(of screen), or features like orientation(potraint, landscape).

Media Queries = "If screen is this size → apply these styles"
```css
/*Syntax */
@media(condition){
}
```
condition is often min & max width
```CSS
/* Mobile devices (portrait) default */
body{
 /* Styles for small screens (e.g., mobile devices) */
}

/* Tablets (landscape phones and small screens) */
@media (min-width: 768px) and (max-width: 1023px) {
  /* Styles for tablets & small laptops*/
}

/* Desktops & laptops */
@media (min-width: 1024px) {
  /* Styles for Desktops & laptops */
}
```
<img width="409" alt="Screenshot 2025-03-14 at 5 27 39 PM" src="https://github.com/user-attachments/assets/10a856b4-5731-4129-8573-ee47fd95fb07" />

