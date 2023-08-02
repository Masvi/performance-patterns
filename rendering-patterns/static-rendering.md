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
