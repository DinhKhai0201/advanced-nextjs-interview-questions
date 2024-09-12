100 advanced Next.js interview questions that cover a wide range of topics, from routing and API routes to performance optimization, deployment, and server-side rendering (SSR).

## Routing & Navigation
### 1. What is the file-based routing system in Next.js, and how does it differ from React Router?
Next.js uses a file-based routing system where every file in the pages/ directory automatically maps to a route. For example, pages/about.js corresponds to the /about route. This is different from React Router, where routes are explicitly defined using components like <Route>. Next.js also supports dynamic and nested routes based on the file and folder structure.

### 2. How do dynamic routes work in Next.js?
Dynamic routes in Next.js are created by using square brackets in the file name. For example, to create a route like /post/123, you would create a file pages/post/[id].js. Inside the component, you can access the dynamic parameter (id) using useRouter() from next/router or getStaticProps.
Example:
```js
// pages/post/[id].js
import { useRouter } from 'next/router';

const Post = () => {
  const router = useRouter();
  const { id } = router.query;

  return <p>Post ID: {id}</p>;
};

export default Post;
```
### 3. How do you implement nested dynamic routes in Next.js?
Nested dynamic routes are created by using folder structures with dynamic file names. For example, if you want a route like /category/[categoryId]/post/[postId], you would create the following structure:
```js
pages/
  category/
    [categoryId]/
      post/
        [postId].js
```
Then, both categoryId and postId can be accessed using useRouter().

### 4. What are catch-all routes, and when would you use them in Next.js?
Catch-all routes in Next.js allow you to match multiple segments of a URL. They are created by adding [...] to the filename, like pages/blog/[...slug].js. This route will match any number of segments, like /blog/a/b/c. Inside the component, slug will be an array of URL segments.

Example:
```js
// pages/blog/[...slug].js
import { useRouter } from 'next/router';

const Blog = () => {
  const router = useRouter();
  const { slug } = router.query; // ['a', 'b', 'c'] for /blog/a/b/c

  return <p>Slug: {slug?.join('/')}</p>;
};

export default Blog;

```

###  5. Explain the difference between shallow routing and normal routing in Next.js.

Shallow routing allows you to change the URL without re-running data-fetching methods such as getStaticProps or getServerSideProps. This is useful for updating the query string or URL parameters without reloading the page content.

Example:
```js
// Using shallow routing
router.push('/about?name=John', undefined, { shallow: true });
```
Normal routing, on the other hand, triggers a full re-render and re-runs data fetching methods.


### 6. How can you implement route prefetching in Next.js?

Next.js automatically prefetches links when they are visible in the viewport using the next/link component. Prefetching means that the page’s resources (like JavaScript) are downloaded in the background so that when the user clicks the link, the page loads faster.

Example:
```js
<Link href="/about" prefetch={true}>About Us</Link>
```
You can also disable prefetching by setting prefetch={false}.
### 7. How do you handle programmatic navigation in Next.js?
You can use the useRouter hook from next/router for programmatic navigation. For example, to navigate to the /about page:

```js
import { useRouter } from 'next/router';

const navigate = () => {
  const router = useRouter();
  router.push('/about');
};

```
You can also use router.replace() to navigate without adding a new entry to the browser's history.

### 8. What are Link and router.push, and when should you use each?
- <Link>: This component is used for declarative navigation and prefetching. It is optimized for internal navigation and triggers Next.js’s routing system.
- router.push: This is used for programmatic navigation (e.g., navigating after form submission or in an event handler).

Use <Link> for static links in your JSX and router.push for more dynamic navigation inside functions or event handlers.

###  9. How can you use middleware in Next.js to handle route-level logic?
Next.js 12 introduced middleware, which runs before a request is completed. Middleware can be used to handle route-level logic such as authentication, logging, or rewriting URLs.

Middleware is created in a _middleware.js or _middleware.ts file inside the pages directory or subdirectories.

Example:
```js
// pages/_middleware.js
export function middleware(req) {
  const { pathname } = req.nextUrl;
  if (pathname === '/secret') {
    return new Response('You are not authorized', { status: 403 });
  }
}
```
###  10. How do API routes work in Next.js, and how can they be used to create a custom backend?
API routes allow you to create a custom API backend within your Next.js application. You can create a new API route by adding a file to the pages/api/ directory. Each file in this folder will map to a unique API endpoint.

Example:

```js
// pages/api/hello.js
export default function handler(req, res) {
  res.status(200).json({ message: 'Hello World' });
}
```
You can handle various HTTP methods (GET, POST, etc.) and even connect to databases or external APIs inside API routes.

###  11. What is the difference between getStaticProps, getServerSideProps, and getInitialProps?
 - getStaticProps: Fetches data at build time and generates static pages. Best for pages where the data doesn’t change frequently.
 - getServerSideProps: Fetches data on each request. This is ideal for data that changes frequently and must be up-to-date for every user request.
 - getInitialProps: This is an older method that can fetch data both client-side and server-side, but it's less efficient compared to the newer methods. It causes slower page loads since data is fetched at runtime on both the server and client.
###  12. What are ISR (Incremental Static Regeneration) and SSG (Static Site Generation), and how do they differ?
SSG: Static Site Generation creates static HTML files during build time for each page.
ISR: Incremental Static Regeneration allows you to update (regenerate) static pages after the site is built, without rebuilding the entire site. This is done by setting a revalidate time in getStaticProps.
ISR is useful when content changes occasionally but doesn't need to be updated on every request, balancing the benefits of static generation with the flexibility of SSR.

###  13. How do you use getStaticPaths in dynamic routing scenarios with SSG?
In dynamic routes, getStaticPaths is used with getStaticProps to pre-generate pages based on dynamic paths. getStaticPaths returns an array of possible values for the dynamic segment.

Example:
```js
export async function getStaticPaths() {
  const paths = [{ params: { id: '1' } }, { params: { id: '2' } }];
  return { paths, fallback: false };
}

export async function getStaticProps({ params }) {
  // Fetch data for the post based on the `id` from `params`
  return { props: { post: {} } };
}
```
###   14. When would you use getServerSideProps over getStaticProps in Next.js?
You would use getServerSideProps when the data is dynamic and needs to be fetched on every request. For example, if the data is frequently updated (like user-specific data, stock prices, or real-time data), getServerSideProps ensures the latest data is fetched on each page load.

###   15. How does data fetching differ between client-side and server-side in Next.js?
Client-side fetching: This happens in the browser after the initial render using hooks like useEffect and libraries like SWR or React Query. It doesn’t block rendering and is useful for data that doesn't need to be available on the initial load.
Server-side fetching: This occurs on the server during page rendering using getStaticProps or getServerSideProps. It ensures that data is available before the page is served to the client.
###  16. How does Next.js handle static exports with getStaticProps?
When using getStaticProps, Next.js statically generates the page at build time. The data is fetched during the build process, and the HTML for the page is generated and cached. No server is involved in serving the page after the initial build, which improves performance and scalability.

###  17. Explain ISR (Incremental Static Regeneration) and its importance in Next.js.
ISR allows you to regenerate static pages after the initial build without needing to rebuild the entire site. With ISR, you can define a revalidate interval in getStaticProps to specify how often Next.js should regenerate the page.

Importance:

You get the performance benefits of static pages while ensuring your data is always fresh.
It helps with scalability by serving static pages but still allows dynamic content updates.
###  18. How does Next.js revalidate a page when using ISR?
When using ISR, you specify a revalidate interval in getStaticProps. After the first user request after the revalidation time expires, Next.js triggers a regeneration of the static page in the background. The updated page will be served to the next user, ensuring that your content stays up-to-date.

###  19. How can you pass data from server to client using getServerSideProps?
getServerSideProps allows you to fetch data on the server and pass it to the component as props. The data is serialized and passed to the React component during the server-side render.

Example:
```js
export async function getServerSideProps(context) {
  const data = await fetchDataFromAPI();
  return { props: { data } };
}
```
###  20. What is getInitialProps, and why is it generally discouraged in favor of newer methods?
getInitialProps was used in early versions of Next.js to fetch data on both the client and server. However, it can introduce performance issues because it forces server-side rendering for all pages, including static ones. It also doesn’t work with automatic static optimization. getStaticProps and getServerSideProps are preferred because they provide more control and better performance.

###  21. What are automatic static optimizations in Next.js?
If a page in Next.js does not use getServerSideProps, getStaticProps, or getInitialProps, Next.js will automatically serve it as a static HTML file. This improves performance because static files can be cached and delivered faster. Automatic static optimization is enabled when Next.js determines a page does not need server-side rendering.

###  22. How does Next.js optimize images using the next/image component?
Next.js provides the next/image component to automatically optimize images. It:

- Supports responsive image loading.
- Automatically serves images in the modern WebP format if supported by the browser.
- Lazy-loads images (only loads them when they are in the viewport).
- Optimizes image size by serving only the required size for the display.
###  23. Explain Next.js automatic code splitting and how it improves performance.
Next.js automatically splits the code by generating separate JavaScript bundles for each page. This means users only download the JavaScript needed for the current page, reducing the overall load time and improving performance. Each page's JavaScript is loaded on demand when the user navigates to that page.

###  24. How do you reduce JavaScript bundle size in a Next.js application?
- Use dynamic imports with next/dynamic.
- Avoid loading large libraries unnecessarily.
- Minimize the usage of polyfills and only load them where needed.
- Tree-shake unused code by ensuring you are using libraries that support tree-shaking.
- Analyze the bundle size using Next.js’s built-in Bundle Analyzer Plugin.
###  25. What is the importance of next/dynamic in improving page performance?
next/dynamic allows you to dynamically import components and load them only when they are needed, reducing the initial bundle size. This is especially useful for large components like charts or modals that may not be needed on the initial page load.

###  26. How can you lazy load components or modules in Next.js?
You can lazy load components in Next.js using next/dynamic. This ensures that the component is loaded only when needed (on-demand), reducing the initial bundle size.

Example:
```js
import dynamic from 'next/dynamic';

const DynamicComponent = dynamic(() => import('../components/HeavyComponent'), { ssr: false });
```
Here, HeavyComponent will only be loaded when it is rendered.

###  27. How do you optimize CSS and static assets in Next.js?
- Use CSS modules or styled-components for scoping styles and reducing global CSS.
- Minify CSS using Next.js’s built-in support.
- Optimize static assets like images using the next/image component, and serve static files from the /public directory.
- Use modern formats like WebP for images.
- Enable caching for static files.
###  28. What is the role of next.config.js in performance optimization?
next.config.js is used to customize and configure a Next.js application. You can use it to:

Enable webpack optimizations like bundle analysis and custom configurations.
Enable advanced caching strategies using headers and rewrites.
Set up custom domains for serving images.
Define environment variables that are optimized during the build.
###  29. How does Next.js handle asset caching?
` automatically adds headers for static assets to be cached efficiently. Assets served from the /public directory have cache-control headers set for long-term caching. For dynamic or regenerated pages, you can control the cache using the revalidate option in ISR or by configuring HTTP headers in next.config.js.

###  30. What is the difference between server-side rendering (SSR) and client-side rendering (CSR) in Next.js, and how do you optimize for each?
SSR (Server-Side Rendering): The HTML is generated on the server for each request, ensuring that the page is rendered with data on the server. It’s used when content must be up-to-date on every request.
CSR (Client-Side Rendering): The HTML is rendered on the client using JavaScript. This is used when content does not need to be available immediately, and data can be fetched after the page is rendered.
To optimize SSR:

- Cache server responses to avoid redundant re-renders.
- Use getServerSideProps selectively for pages that require dynamic data.
  
To optimize CSR:

- Load only the necessary JavaScript for the initial render (code splitting).
- Prefetch data and use caching strategies (like useSWR or React Query).

