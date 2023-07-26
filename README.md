# Performance patterns

Performance patterns can be used to achieve a better user and developer experience.

Any client-side JavaScript in our application has to be shipped to the client in one way or another. Before this can happen, we have to make sure that:

1. The JavaScript is executable in a browser environment
2. The file that the browser has to download and fetch only contains relevant code, to ensure the browser can quickly fetch this bundle without using too much bandwidth

---
### Bundlers

A bundler bundles our application together in one or multiple files, and makes it possible to make code executable in other environments, for example browsers. A bundler receives an `entry` file, from which it starts to bundle the code together. If we’ re importing modules from other files, the bundler traverses these modules in order to include them all in the bundle. 

### Compilers

A compiler converts JavaScript (or Typescript) code into another version of JavaScript, which could be backwards compatible in current and older browser or environments.

A compiler does *not* bundle code. It simply transforms it to another version, based on a certain configuration.

### Minifiers

A minifier can reduce the size of a JavaScript file based on a certain configuration, for example by removes comments, making variable and function names smaller, removing white space. and do on.

### Bundle Splitting

Is the process of creating multiple, smaller bundles rather than on large bundle. 

A larger bundle can lead to an increased amount of loading time, processing time, and execution time. Users on low-end devices or slower networks will see a significant increase in loading time before the bundle has been fetched. 

To avoid larger bundles, we can tell bundler to create multiple, smaller bundles, instead of bundling everything into one  big file.

### Tree Shaking 

With tree shaking, we can reduce the bundle size by eliminating dead code. Tree shaking is aimed at removing unused code from a JavaScript bundle. Bundlers can automatically detect dead code, to exclude this code from the final bundle. 
For example, if two methods are exported from the `input.js` file, namely `validateInput`and `formatInput`, but we're only importing `validateInput`, the bundler will ensure that the `formatInput` method won't be included in the final bundle.

```js
// input.js
export function validateInput(input) {
  const isValid = input.length > 10;
  return isValid;
}

export function formatInput(input) {
  const formattedInput = input.toLowerCase();
  return formattedInput;
}
```


```js
// index.js
import { validateInput } from "./input";

const input = document.getElementById("input");
const btn = document.getElementById("btn");

btn.addEventListener("click", () => {
  validateInput(input.value);
});
```
After tree shaking, the final bundle won't include the `formatInput` function as it's no referenced in the code.

