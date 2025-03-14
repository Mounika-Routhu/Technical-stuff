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

## strong vs em vs mark vs i
<img width="897" alt="Screenshot 2025-03-14 at 12 58 37 PM" src="https://github.com/user-attachments/assets/65c4bfd1-9b04-4a86-83f5-cc27cfde0242" />

## New feature in HTML 5
1. **New Semantic Elements** − These are like `<header>, <footer>, and <section>`.
2. **Forms 2.0** − Improvements to HTML web forms where new attributes have been introduced for <input> tag.
   - New input types: `email, tel, url, number, date, range`, etc., which provide better validation and user input behavior . Also `Placeholder`,`Autofocus`, `Required`  
3. **Local Storage & Session Storage** − To achieve without resorting to third-party plugins.
4. **WebSocket** − A next-generation bidirectional communication technology for web applications. For **chat, game apps real time wihtout page refresh**
5. **Server-Sent Events(SSE)** − allow the server to push updates to the client. For **live stocls - real time wihtout page refresh**
6. **Canvas** − This supports a two-dimensional drawing surface that you can program with JavaScript.
7. **Audio & Video** − You can embed audio or video on your webpages without resorting to third-party plugins.
8. **Geolocation** − Now visitors can choose to share their physical location with your web application.
9. **Microdata** − This lets you create your own vocabularies beyond HTML5 and extend your web pages with custom semantics. - **for better SEO**
10. **Drag and drop** − Drag and drop the items from one location to another location on the same webpage.

# Comparison Summary: **localStorage / sessionStorage** vs. **Cookies**

| **Feature**                | **localStorage / sessionStorage**                        | **Cookies**                                              |
|----------------------------|----------------------------------------------------------|---------------------------------------------------------|
| **Storage Size**            | Typically **5-10 MB** per origin(10 MB for microsoft edge) | Limited to **~4 KB** per cookie                         |
| **Automatic Transmission**  | **No** - Data is not sent automatically with HTTP requests | **Yes** - Sent automatically with every HTTP request to the server(can be restricted) |
| **Persistence**             | **localStorage**: Persistent across sessions; **sessionStorage**: Cleared when the tab/browser is closed | Can be persistent (until expiration) or session-based |
| **Ease of Use**             | Simple API (`setItem()`, `getItem()`, `removeItem()`)    | More complex, requires managing expiration and flags  `document.cookie = "username=john_doe; domain=example.com";`  |
| **Data Type**               | Suitable for larger data (objects, arrays, strings)      | Typically used for small, key-value data (session IDs, preferences) |
| **Security**                | Vulnerable to **XSS** (JavaScript can access stored data) | Vulnerable to **XSS** (but can be made secure with flags like `HttpOnly`, `Secure`, `SameSite`) |
| **Expiration**              | **localStorage**: No expiration, persits page load, same origin tab can access;<br> **sessionStorage**: Clears when session ends, **don't** persits page load, same origin tab **can't** access | Expiration is managed manually with `expires` or `max-age` attributes. persits page load, same origin tab can access data but subjected to expire |
| **Usage in SPAs**           | Well-suited for **Single-Page Applications (SPAs)** to manage state between pages | Less suited for SPAs due to automatic transmission on every request |
| **Data Storage Location**   | **Client-Side** | Stored both client-side and server-side (if sent with requests) |
| **Performance**             | Faster, as data is stored and accessed client-side without network overhead | Potential overhead due to automatic sending with each request |

**Note:**
1. `localStorage, sessionStorage`, and `cookies` are all **domain and protocol-specific** by **default**. This is due to the **Same-Origin Policy(SOP)**,
2. **SOP** which is a fundamental security feature in web browsers by **default** that restricts how documents or scripts from one origin can interact with resources from another origin.

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
