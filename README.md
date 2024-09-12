100 advanced Next.js interview questions that cover a wide range of topics, from routing and API routes to performance optimization, deployment, and server-side rendering (SSR).

## Routing & Navigation
### 1. What is the file-based routing system in Next.js, and how does it differ from React Router?
Next.js uses a file-based routing system where every file in the pages/ directory automatically maps to a route. For example, pages/about.js corresponds to the /about route. This is different from React Router, where routes are explicitly defined using components like <Route>. Next.js also supports dynamic and nested routes based on the file and folder structure.

### 2. How do dynamic routes work in Next.js? Explain with an example.
Dynamic routes in Next.js are created by using square brackets in the file name. For example, to create a route like /post/123, you would create a file pages/post/[id].js. Inside the component, you can access the dynamic parameter (id) using useRouter() from next/router or getStaticProps.
Example:
```
// pages/post/[id].js
import { useRouter } from 'next/router';

const Post = () => {
  const router = useRouter();
  const { id } = router.query;

  return <p>Post ID: {id}</p>;
};

export default Post;
```

