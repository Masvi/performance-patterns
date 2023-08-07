# Incremental Static Regeneration

Pre-render certain pages, and render the other pages on-demand.

Static rendering comes with many diferents performances benefits, but it can up in long build times if we have many pages to pre-render. We can also
only update the content of our page by redeploying the website, which isn't a great user experience. 

Incremental Static Generation allows us to only pre-render a subset of pages, for example pages that are likely to be requested by the user, and 
render the rest on-demand. When the user requests a page that hasn't been pre-rendered yet, the page gets server-rendered, after which can get 
cached by a CDN.

Besides only pre-rendering a subset of pages, we can automatically invalidate cached pages based on a `stale-while-revalidate` approach. When a user
requests a page that is stale - meaning it's been in cache for longer than the provided number that it should be cached for - a regeneration is triggered 
in the background. While this is happing, the user gets to see the stale page, but they're able to see the updated content on subsequent requests. 

### Implementation

We can implement Incremental Static Regeneration using Next.js's `getStaticProps` method in combination with the `getStaticPaths` method.

```js
import { Listings, ListingsSkeleton } from "../components";

export default function Listing(props) {
  return <ListingLayout listings={props.listing} />
}

export async function getStaticProps(props) {
  const res = await fetch(`https://my.cms.com/listings/${props.params.id}`);
  const { listing } = await res.json();

  return { props: { listing } }
}

export function getStaticPaths() {
  const res = await fetch(`https://my.cms.com/listings?limit=20`);
  const { listings } = await res.json();

  return {
    params: listings.map(listing => ({ id: listing.id })),
    fallback: false
  }
}
```
