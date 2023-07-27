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

