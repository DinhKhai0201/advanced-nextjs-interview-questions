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



