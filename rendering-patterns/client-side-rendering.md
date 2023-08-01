# Client-Side Rendering 

Render your application's UI on the client.
The contents of a client-side rendered application get rendered in the browser
- Client requests HTML from server
- Server returns barebones HTML
- Browser parses and renders HTML
- Client requests and downloads JavasCript
- Browser executes JS, renders content

When a user requests a client-side rendered application, the server initially responds with the barebones HTML file. Once the client recieves this HTML file,
the HTML parser parses the content, and fetches the JavaScript bundle when it reaches the `script` tag.

When the client has downloaded the JavasScript, it executes it contents. This contains DOM methods to dynamically append content to the DOM tree, which 
results in rendered content to the user's screen.

### Implementation

A basic client-side application consist of at least two file.
First, we need to have a barebones HTML file, which contains the element that the JavaScript file can use to dynamically append content to.

```html
<html>
  <body>
    <div id="root"></div>
    <script src="/bundle.js"></script>
  </body>
</html>
```
We also need a JavaScript file, which contains methods to update the DOM tree and dynamically render data. This file gets fetched after (or during) the HTML
parsing.
```js
const root = document.getElementById("root");

// DOM manipulation
root.appendChild(...)
```

### Tradeoffs

- `TTFB`: The Time To First Byte can be fast, since the initial HTML does not contain large components.
- `FCP`: The First Contentful Paint can occur once the JavaScript bundle has downloaded, parsed, and executed its contents.
- `TTI`: The Time To Interactive can occur once the JavaScript bundle has downloaded, parsed, and executed its contents to bind the event handlers to the components.
- `LCP`: The Largest Contentful Paint can occur at the same time as the First Contentful Paint, provided there aren't any large components such as large images or videos.


<b>Interactivity:</b> The rendered content is instantly interactive. The fetched JavaScript bundle can directly add Event Listners to the DOM nodes. As opposed to
other rendering patterns, users are never left with a visible but non-interactive page.

<b>Single server roundtrip:</b> The entire web application is loaded on the first request. 

<b>Bundle size:</b> Modern applications, which usually contain multiple pages, can easily have large JavaScript bundles. The larger the bundle, the longer
it takes to download and execute the JavaScript before the first content is visible and interactive. 

<b>SEO:</b> Large bundles and waterfall of API requests may result in content no being rendered fast enough for a crawler to index it. Workarounds are required to make a client-rendered website 
SEO friendly, and work around the limitations of a crawler's ability to understand JavaScript.





