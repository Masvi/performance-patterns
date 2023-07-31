# Import on Visibility

Load non-critical components when they are visible in the viewport.
We saw how we can dynamically import components base on user interaction. However, we can also dynamically import components based on their
visibility within the viewport.

For example, if we wanted to show the listings on smaller viewports, not all listigns are instantly visible to the user. Instead, we can lazy-load the 
listings, and only load them when they're visible in the viewport when the user scrolls down.

### Implementation

One way to dinamically import components on interaction is by using the [Intersections Observer API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API). 
There's a React hook called `react-intersection-observer` that we can use to easily detect whether a component is visible in the viewport.

Lazy-loading the `Footer` component would result in something like this: 

```js
import { Suspense, lazy } from "react";
import { useInView } from "react-intersection-observer";
const Listing = lazy(() => import("./components/Listing"));

function ListingCard(props) {
  const { ref, inView } = useInView();

  return (
    <div ref={ref}>
      <Suspense fallback={<div />}>{inView && <Listing />}</Suspense>
    </div>
  );
}
```
