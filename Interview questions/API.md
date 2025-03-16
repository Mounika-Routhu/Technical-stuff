API
HTTP
REST API
fetch
axios
why axios is better
request headers
Response error codes
CORS
Web Security Best Practices
XSS
sanitize input=>dompurity/ custom function

```js
import * as DOMPurify from 'dompurify';
const dirtyHtml = '<p>Hello <img src="image.jpg" onerror="alert(\'XSS\')"> world!</p>';
const cleanHtml = DOMPurify.sanitize(dirtyHtml, {
  ALLOWED_TAGS: ['img'],
  ALLOWED_ATTR: ['src']
});
console.log(cleanHtml); // Output: <p>Hello <img src="image.jpg"> world!</p>
```
Content-Security-Policy: default-src 'self'; script-src 'self' https://trustedsource.com;
, clickjacking
x-frame-options: DENY /self
, and MIME sniffing.
X-Content-Type-Options: nosniff
