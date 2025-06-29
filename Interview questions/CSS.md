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
A pseudo-class selector targets an element based on its **state or position** in the DOM 
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
| **12. Pseudo-element Selector** | Targets **parts of an element** (like the first letter or line).    | `p::first-letter { font-size: 200%; }`        |

