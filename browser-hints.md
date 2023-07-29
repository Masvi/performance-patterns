# Browser Hints

Use hints to inform the browser about (optionally) critical resources.

### Prefetch

Fetch and cache resources that my be requested some time soon.

The `prefetch` browser hint can be used to fetch resources that may be needed some time soon, but not immediately on the initial load. 
This can be the case on subsequent requests or page navigations that a user is likely to make.
A prefetched resource is fetched when the browser is idle and has calculated that it's got enough bandwidth, after which it caches the prefetched 
resource. When the client actually needs the resource, it can easily get it from cache instead of having to make a request to the server.

For example if we use [route-based splliting](https://github.com/Masvi/performance-patterns/blob/main/route-based-splitting.md), and we know that 
most users will navigate to the `/about` route, we can prefetch this route to get a faster navigation, resulting in a better UX. Instead
of waiting for the user interaction to fetch the `about.bundle.js` bundle, the browser prefetched this resource when it was idle. Once the user actually 
navigates to the `/about` page, the bundle can quickly load from cache instead of making a request to the server

### Implementation

We can prefetch a resource by explicitly adding it to the `head` of the `html` document.

```html
<link rel="prefetch" href="./about.bundle.js" />
```

If you're using Webpack, you can prefetch it dynamically by using the `/* webpackPrefetch: true */` magic comment. 

```js
const About = lazy(() => import(/* webpackPrefetch: true */ "./about"));
```

```js
import React, { lazy, Suspense } from 'react';
import { createRoot } from 'react-dom/client';
import {
  Routes,
  Route,
  BrowserRouter as Router,
  Link,
  Outlet,
} from 'react-router-dom';

const App = lazy(() => import('./pages/App'));
const About = lazy(() =>
  import(/* webpackPrefetch: true, webpackChunkName: "about" */ './pages/About')
);
const Contact = lazy(() => import('./pages/Contact'));

export function Nav() {
  return (
    <div>
      <nav>
        <h1>
          <Link to="/">
            <span>🏡</span> Houses.
          </Link>
        </h1>
        <ul>
          <li>
            <Link to="/about">About</Link>
          </li>
          <li>
            <Link to="/contact">Contact</Link>
          </li>
        </ul>
      </nav>
      <Outlet />
    </div>
  );
}

createRoot(document.getElementById('root')).render(
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Routes>
        <Route path="/" element={<Nav />}>
          <Route
            index
            path="/"
            element={
              <React.Suspense fallback={<div />}>
                <App />
              </React.Suspense>
            }
          />
          <Route
            path="/about"
            element={
              <React.Suspense fallback={<div />}>
                <About />
              </React.Suspense>
            }
          />
          <Route
            path="/contact"
            element={
              <React.Suspense fallback={<div />}>
                <Contact />
              </React.Suspense>
            }
          />
        </Route>
      </Routes>
    </Suspense>
  </Router>
);

```
