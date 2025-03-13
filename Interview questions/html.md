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
