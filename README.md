# NextjsNotes

## How Next.js works

Next.js provides a framework to structure your application, and optimizations that help make both the development process and final application faster while react doesn't provide features like that. 
Let's discuss features provided by next.js which helps to make everything better.

### From development to Production 

- Development needs : `Typescript support , ESLint Integration , Fast Refresh` and More 
- Production Needs : `Compiled  , bundled , minified and code split`

The next.js compiler does these things very well , everything is done very smoothly because next.js compiler is written in a low level programming called `Rust` and another platfrom `SWC` for compilation , minification , bundling and more.

### Compiling 

- Compiling simply converts `JSX , Typescipt , modern js ` code into plain Javascipt .
- Understand by this image [Code from higher langauge to plain js ](https://nextjs.org/static/images/learn/foundations/compiling.png)
- In Next.js, compilation happens during the development stage as you edit your code, and as part of the build step to prepare your application for production.

_Quick Review: Why does your web application code need to be compiled?
Answer : Code needs to be transformed into a version browsers can understand_

### Minifying 

- Developer write code understandable by humans. Adds many things like comments, spaces, indents, and multiple lines.
- In minification process we remove all that , and minify the code to reduce the size for production level.
- Next.js automatically minify JS and CSS . 
- [See example](https://nextjs.org/static/images/learn/foundations/minifying.png)

_ Quick Review:  Minifying removes syntax highlighting from your code?
Answer : False - syntax highlighting is provided by your code editor _

### Bundling 

- Bundling is the process of resolving the web of dependencies and merging (or ‘packaging’) the files (or modules) into optimized bundles for the browser, with the goal of reducing the number of requests for files when a user visits a web page.


_ Quick Review:  What is the difference between compiling and bundling in web development?
Answer : Compiling is transforming code into something parsable by browsers. Bundling is resolving your applications dependency graph and reducing the number of files _

### Code Spliting 

- Split the into chunks so that we access only neeeded chunk of a code not the whole code. It improve performance.
- Next.js automatically do that. Each file in Pages/ folder becomes a chunks itself.

`Important to know `
- Shared code becomes bundle , so that it doesn't load on again.
- On Initial load , next.js can pre-load the code of another pages according to the need.
- Dynamic imports to load initially required code.


### Build time and Runtime

- `Build time (build step)` , a series of step to prepare prdc. build.
- Production ready files : 
   - HTML files for static pages
   - js code for rendering on server side
   - js for client side
   - CSS files
- `Runtime` means required time to response on a user request by your application after app is deployed and built.


### Rendering

- Converting the code from React to HTML representation of your UI is called rendering.
- Rendering can take place `on the server` or `on the client`. It can happen either ahead of time `at build time`, or `on every request at runtime`. 
- With Next.js, three types of rendering methods are available: `Server-Side Rendering, Static Site Generation, and Client-Side Rendering`.

#### Pre-rendering :
  - Server-Side Rendering and Static Site Generation  is called Pre-rendering because the fetching of external data and transformation of React components into HTML happens before the result is sent to the client.  

#### client side rendering vs pre-rendering
  - Client side rendering means the browser receives an empty HTML shell from the server along with the JavaScript instructions to construct the UI. This is called client-side rendering because the initial rendering work happens on the user's device.
  - React does client side rendering , but in Next we can do both.
  - use `useEffect` and `useSWR` for client side rendering in Next.js

  - In pre-rendering , user will se constructed page on initial render .
  - on initial render there will be only HTml on interactivity
  - after Hydration , interative UI take form. [Understand by example ](https://nextjs.org/static/images/learn/foundations/pre-rendering.png)

  Two Types of pre-rendering :-

  `1.Server side` : 
     - generate HTml file , json data and Javascript instruction on server and sent to client.
     - First non-interative HTML will load , than react use json data and instruction to add interactivity , this process is called hydration.
     - we can do server side rendering by `getServerSideProps` in Next.js
     - React 18 also introduce `server components` which will load on server side , also you can perform some logic on server side and sent only result to reduce bundle size.

   `2.Static side Generation`:
     - unlike server side at runtime , we don't generate HTML .
     - Generate once at build time and store it in CDN's .
     - we use `getStaticProps` in next.js for static generation.
     - Note: You can use `Incremental Static Regeneration` to create or update static pages after you’ve built your site. This means you do not have to rebuild your entire site if your data changes.  

The beauty of next.js is that you can choose any method at any page in single application.



### CDNs and Edge

- CDN store static content like HTML and images e.t.c to nearby location server.
- Edge can perform operation too on nearby server. It can run small snippets of code. 
- Move some work on edge and it will make your app more performant because it reduce amount of code sent to the client.
- You can run code at edge with `middleware` , soon with `react server components`
- [understand better by examples](https://vercel.com/features/edge-functions#:~:text=Unlock%20the%20potential%20of%20edge%20computing)



## Routing in Next.js

- Next.js supports file system based routing.

### Pages, templates and Layouts 

- from Next.js 13 , we can make pages and can use shared layout and templates

- #### Page : 
    - page.js is a file which denotes a unique route , the name of the route should be the name of baseUrl till that page.js file.
    - For example : app/dashboard/page.js , then the url will be {base_url}/dashboard 
    - Pages are server components by default but we can convert them into client side components.

- #### Layout : 
    - A common UI accessed by it sibling and child pages.
    - They preserve state, remain interactive , do not render on route change.
    - can also be nested 
    - make a layout by default export of layout.js from a folder , that layout will work inside that folder. No need to wrap the page inside it. 
    - layout({children}) , children prop is needed.
    - Server side components default in nature , but can be convert in client side components
    - Nesting is allowed in layouts
    - there is also a root layout which is accessed by every page. 

- #### Templates : 
    - Templates are same as Layouts but,
    - Templates creates new instances on route change.
    - They don't preserve state , effects.


## Linking and Navigating

- Two way of linking:
   - `<Link>`
   - `useRouter`
- `usePathname()` is a hook which gives current active link.
- You can scroll to an id in the UI by passing id with # in url Ex: ( {base_url}/page#conclusion)
- When you go on new route , next scroll you on top , you can hide this behaviour by passing {scroll:false} to link , router.push() or router.replace() methods.

- `useRouter` can only be use in client components.

### Working of Linking and navigation

1. `Prefetching` :
      - Preload a route in background before user visit
      - When the link of a route comes in a viewport , prefetch the whole page.
      - Two ways of prefetching 
          - `<Link>` : Automatically prefetch as they become visible to viewport.
          - `router.prefetch()` : can be used to prefetch programatically.

      - Link has diffrent behaviors for static and dynamic routes;
          - `static Routes` : prefetch default to true, all routes is prefetched and cached.
          - `Dynamic Routes` :  prefetch default to automatic. Only the shared layout down until the first loading.js file is prefetched and cached for 30s. This reduces the cost of fetching an entire dynamic route, and it means you can show an instant loading state for better visual feedback to users.
    
      - Disable prefetching by prop to `false`
      - Pre-fetching only works in production

2. `Caching` : cache is used to reduced same request to the server , Next.js cache the data of request of every route

3. `Partial rendering` : Only segment which is changed will render on a route , rest will preserve. For Example layout will preserve.

4.  `Soft Navigation` : Don't reload the whole page , only renders the segments that have changed.

5. Backword and forward navigation is also suppported.


## Route groups : 

- Group the similiar route in a group folder without affecting the routes URl
- For doing this use parenthesis () , for example folder name -> (marketing) , consist all the route related to marketing , but it will not add in route url.
- Diffrent group folder can have diffrent layouts on the same level of heirarchy . For example [Look at this example ](https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Froute-group-organisation.png&w=1920&q=75&dpl=dpl_8QhM23JmjceptNMFdCFKaKX6dhHD)

- This can also help to use a single layout on multiple routes which is related to each other. Just pass the layout in `Route group folder`. [You can check this image ](https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Froute-group-opt-in-layouts.png&w=1920&q=75&dpl=dpl_8QhM23JmjceptNMFdCFKaKX6dhHD)
- You can also create multiple root layout , just remove root layout and add a unique layout in every `Route group folder` according to your needs.
- Naming of route group had no specific reason , it is just to organise
- Two or more Route Group can not have same route urls ( for example : (marketing)/about/page.js and (shop)/about/page.js would both resolve to /about and cause an error.)
- If you are using multiple root layout , then your home page.js should be in any route group.
- Navigtion between multiple root layout can cause `full page load`.


## Dynamic Routes : 

- When you don't know the name of route , for example a route which is genrated dynamically.
- Convention : [Foldername] , i.e. [id] or [slug]
- For generating static params at build time instead of on-demand,we use generateStaticProps().
- #### `catch-all` segments : 
       - use [...foldername] for more variable in route; 
        Route	Example URL	params
        app/shop/[...slug]/page.js	/shop/a	{ slug: ['a'] }
        app/shop/[...slug]/page.js	/shop/a/b	{ slug: ['a', 'b'] }
        app/shop/[...slug]/page.js	/shop/a/b/c	{ slug: ['a', 'b', 'c'] }

- #### `optional catch-all` segments :
      - This also match route with zero slug , if slug : [] , then it will indicate app/shop. This is the diffrence between option catch-all and catch-all.
      







