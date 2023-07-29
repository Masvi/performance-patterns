# Route Based Splitting 

Dynamically load components based on the current route. 
If your application has multiple pages, we can use dynamic imports to only load the resources that are needed for the current route. 
Instead of the code for all the possible pages in the initial bundle, we can bundle-split based on routes. This approach allows us to defer 
loading the bundle until the user actually navigates to that page.


### Implementation

If you're using `react-router` for navigation, you can wrap the `Switch` component in a `React.Suspense`, and import the routes 
using `React.lazy`. This automatically enables route-based code splitting.

```js
import React, { lazy, Suspense } from "react";
import { Switch, Route, BrowserRouter as Router } from "react-router-dom";

const App = lazy(() => import("./App"));
const About = lazy(() => import("./About"));
const Contact = lazy(() => import("./Contact"));

ReactDOM.render(
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/">
          <App />
        </Route>
        <Route path="/about">
          <About />
        </Route>
        <Route path="/contact">
          <Contact />
        </Route>
      </Switch>
    </Suspense>
  </Router>,
  document.getElementById("root")
);
```

