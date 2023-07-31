# Rendering Patterns 

Rendering content on the web can be done in many ways. The decision _how_ and _where_ to fetch and render content is key to the performance of an application.
The available frameworks and libraries can be used to implement different rendering patterns like Client-Side Rendering, Static Rendering, Incremental Static Regeneration, 
Progressive Rendering, Server-Side Rendering and many more. Understanding the tradeoffs and use cases for these patterns can drastically help the performance of your
application, resulting in a great user and developer experience. 


### Web Vitals

To measure how well our website performs, we can use a set of useful measurements called Web Vitals. A subset of these measurements - the Core Web Vitals - is usually
used to determine the performance of a page, and can affect your website's SEO. 

`TTFB` - Time it takes for a client to receive the first byte of page content

`FCP` - Time it takes to the browser to render the first piece of content after navigation

`LCP` - Time it takes to load and render the page's main content

`TTI` - Time from when the page starts loading to when it's reliably responding to user input quickly 

`CLS` - Measures visual stability to avoid unexpected layout shift

`FIP` - Time from when the user interacts with the page to the time when the event handlers are able to run
