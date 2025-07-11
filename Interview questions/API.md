## AJAX - FETCH/AXIOS - HTTP/HTTPS - REST API
1. AJAX is a technique that allows a client (usually a web application) to send or receive data asynchronously from a server, without refreshing the entire page.
2. AJAX uses tools like fetch - A JS built in API to implement AJAX, axios - Library for AJAX (easier than fetch)
3. The communication happens over the HTTP(HyperText Transfer Protocal), which uses various methods like GET, POST, PUT, PATCH, and DELETE to interact with resources on the server.
4. In modern applications, this is often done over HTTPS — the secure version of HTTP — which ensures that all data transmitted is encrypted using an SSL/TLS certificate. This creates a secure tunnel between the client and the server, protecting the data from attackers or man-in-the-middle threats.
5. A REST API (Representational State Transfer API) is a server that exposes endpoints which can be accessed using HTTP methods. These endpoints allow clients to perform CRUD operations:
   1. GET — retrieve data
   2. POST — create data
   3. PUT/PATCH — update data
   4. DELETE — remove data

```sql
+------------------+          AJAX (fetch / axios)         +------------------+
|  React App (UI)  |   --------------------------------->   |   REST API Server |
|  (Browser)       |        HTTP / HTTPS Request           |  (Node, Django, etc.)
+------------------+                                       +------------------+
        |                                                       |
        |          JSON Response (data)                         |
        |   <-----------------------------------------------     |
        |                                                       |
        v                                                       |
+------------------+                                            |
| Update React UI  |                                            |
| with new data    |                                            |
+------------------+                                            |

```

## CDN - Content delivery network
1. A CDN is a network of distributed servers placed around the world that deliver content (like images, CSS, JS, videos, etc.) faster to users based on their geographic location.
2. In simple terms, instead of serving all your website files from one central server, a CDN copies and stores those files in many servers worldwide.
3. So when a user visits your site: They get the content from the nearest CDN server, making your site load faster and reduce server load.
4. for example, person in india tries they will get from mumbai server or delhi server. US person tries he will get from nearest US server
5. Uses are, faster, caching, gloabal access - availability, scalability(maintainable) 

```HTML
<img src="https://your-website.com/assets/mounika.jpg" alt="Profile" /> // local

<img src="https://cdn.jsdelivr.net/gh/username/images/mounika.jpg" alt="Profile" /> // CDN
```

API
HTTP
REST API
fetch
axios
why axios is better
request headers
Response error codes
CORS
Web Security Best Practices - XSS, click jacking, CORS
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
