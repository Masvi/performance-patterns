# Dynamic Imports

Statically imported modules are all included in the final bundle of our app, even components that don't need to be rendered right away. 
Depending on the size of the components and the final bundle, this could lead to a worse initial loading experience, as the 
client has to download and parse the entire bundle.

In many cases, we can defer the import of modules until they're actually needed, which results in smaller bundles.

### Use case

Let's say for example that we have a `Search` input component. When a user clicks on the search input, we show a `SearchPopUp` 
component that shows some popular locations. The `SearchPopUp` component isn't instantly visible on the screen - or maybe won't even 
be visible at all if the user never clicks on the `SearchInput`. We can dynamically import the `SearchPopUp` component, which separates 
this code from the initial bundle, and creates a 
separate bundle for just this component.

### Implementation

In React, we can dynamically load a component by using `React.Suspense` with `React.lazy`. The `Suspense` component receives a `fallback`, 
that gets rendered while the client is fetching the `SearchPopUp` bundle

In this example, `Card1` and `Card2` are statically imported and included in the initial bundle. `Card3` and `Card4` however are dynamically loaded on user interation.

```js
import React, { Suspense, lazy } from 'react';
import './styles.css';

import { Card } from './components/Card';
import Card1 from './components/Card1';
import Card2 from './components/Card2';
const Card3 = lazy(() =>
  import(/*webpackChunkName: "card3" */ './components/Card3')
);
const Card4 = lazy(() =>
  import(/*webpackChunkName: "card4" */ './components/Card4')
);

const App = () => {
  return (
    <div className="App">
      <Card1 />
      <Card2 />
      <DynamicCard component={Card3} name="Card3" />
      <DynamicCard component={Card4} name="Card4" />
    </div>
  );
};

function DynamicCard(props) {
  const [open, toggle] = React.useReducer((s) => !s, false);
  const Component = props.component;

  return (
    <Suspense fallback={<p id="loading">Loading...</p>}>
      {open ? (
        <Component />
      ) : (
        <Card rendered={false} onClick={toggle}>
          <p>
            Click here to dynamically import <code>{props.name}</code> component
          </p>
        </Card>
      )}
    </Suspense>
  );
}

export default App;

```
