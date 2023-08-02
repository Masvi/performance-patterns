# Static Rendering

Deliver pre-rendered HTML that was generated when the site was built. 

With static rendering, the HTML contents are pre-generated at build time. When a user requests a statically
rendered application, the server responds with the HTML file. 
Once the client receives this HTML file, the HTML file parser parses the content and renders the non-interactive content
to the screen. If a `script` tag is present, the client sends an additional request to fetch this bundle. 
When the client has downloaded the JavaScript, it executes its contents and adds event listeners to the HTML elements to make them
interactive. 

- Client requests HTML from server
- Server returns requested HTML
- Browser parses and renders content
- Client requests JS bundle from server
- Browser hydrates elements

When a user requests a statically rendered application, the server responds with the HTML file. 

Once the client receives this HTML file, the HTML parser parses the content and renders the non-interactive content to the screen. 
If a `script` tag is present, the client sends an additional request to fetch this bundle. 
When the client has dowloaded the JavaScript, it executes its contents and adds event listeners to the HTML elements to make them 
interactive. 

### Implementation

The bare minimum required for a static rendered application is to have one single HTML file that contians all the contents of the 
elements that need to be rendered on the screen.

```js
const listings = [
  { id: 1, address: "..." },
  { id: 2, address: "..." },
];

export default function Home() {
  return <Listings listings={listings} />;
}
```

There can also be an optional JavaScript file, which is only necessary if the components are interactive, to bind event
listeners to the already rendered HTML elements.

### Tradeoffs

Performance
- `TTFB:` The time to fisrt byte can be fast, since the initial HTML does not contain large components.
- `FCP:` The first contentful paint can occur once the JavaScript bundle has downloaded, parsed, and executed its contents.
- `TTI:` The time to interactive can ocrrur once the JavaScript bundle has downloaded, parsed, and executed its contents to bind the event handlers to the compoents.
- `LCP:` The largest contentful paint can occur at the same time as the first contentful paint, provided there aren't any large components such as large images or videos.

<b>Cacheability:</b> Pre-rendered HTML files can be cached and served by a global CDN. Users can benefit from quick responses when they request a page, as the request doesn't have to go all the way to the origin server.

<b>SEO:</b> The HTML content can be rendered by web crawlers with no extra effort.

<b>Availability:</b> Static is always online. Even if your backend or data source (e.g. database) goes down, your existing pre-rendered page will still be available.

<b>Backend load:</b> With Static Generation, the database or API wouldn't need to be hit on every request. Page-rendering code wouldn't have to run on every request.

<b>Dynamic data:</b> If a statically generated page needs dynamic content, for example from an external data source, it needs to fetch this client-side. This can result in a long LCP, and higher server costs.
