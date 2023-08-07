# Server-Side Rendering 

With Server-Side rendering, we can generate HTML on the server (or serverless function) on every request.

When a user request a server-side rendered application, the server generates the HTML, and returns this to the client.
The browser renders this content, which is initially just plain non-interactive HTML elements. To bind event listeners 
to the components, the client sends an additional request to fetch the JavaScript bundle to hydrate the components.

### Implementation

When server-side rendering an applicantion, we need a method to render HTML from our React components on the server, and 
hydrate the non interactive HTML on the client. One way to render HTML on the server, is by using the `renderToString` method.
This function returns HTML string corresponding to the React element. The HTML can then be rendered to the client
for a faster page load, and hydrated with the `hydrateRoot` method on the client.

When using Next.js, we can server-render a page by using getServerSideProps method.

```js
import { Listings, ListingsSkeleton } from "../components";

export default function Home(props) {
  return <Listings listings={props.listings} />;
}

export async function getServerSideProps({ req, res }) {
  const res = await fetch("https://my.cms.com/listings");
  const listings = await res.json();

  return {
    props: { listings },
  };
}
```

This methods run on every request, and passes the props to the page to be dynamically generated.

### Tradeoffs

- `TTFB`: The TTFB can be slow, since the page still has to be generated on-demand.
- `FCP`: The First Contentful Paint can occur once the HTML has been parsed and rendered.
- `LCP`: The Largest Contentful Paint can occur at the same time as the First Contentful Paint, provided there aren't any large components such as large images or videos.
- `TTI`: The Time To Interactive can occur once the HTML has been rendered, and the JavaScript bundle has been downloaded, parsed, and executed its contents to bind the event handlers to the components.

- <b>Personalized pages:</b> Server-rendering is useful for pages that need request-based data, such as a user cookir.
- <b>Render blocking:</b> Server-rendering can block the generation of pages that are authetication-based.


`Initial load`: Since the page still has to be generated when the user requests it, it can take a while before the user can see something rendered on their screens. To optimize SSR, you can:

1. Optimize database queries. If the distance between your server and database is long, it can take a while to establish a connection and retrieve data. Consider moving your database or server closer to each other.
2. Add Cache-Control headers to your responses.
