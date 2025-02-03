# **Detailed Notes of React Query.**

## **Defination**

React Query is a data-fetching and state management library for React applications. It simplifies data fetching, caching, synchronization, and background updates, making it easier to manage server state efficiently.

### Key Features:

- **Automatic Caching**: Stores fetched data and reuses it when needed.
- **Background Updates**: Fetches fresh data in the background without blocking UI.
- **Synchronization**: Keeps client and server data in sync automatically.
- **Pagination & Infinite Queries**: Provides built-in support for paginated and infinite scrolling data.
- **Mutation Handling**: Allows easy data modifications (POST, PUT, DELETE) with automatic revalidation.
- **DevTools**: Comes with a powerful debugging tool to inspect queries and mutations.

It helps reduce boilerplate code compared to manual `useEffect` and `useState` solutions, improving performance and developer experience.
s

## **Setting Up React Query in React 18**

### **1. Install React Query**

Run the following command in the terminal:

```bash
npm install @tanstack/react-query
```

### **2. (Optional) Install React Query DevTools**

```bash
npm install @tanstack/react-query-devtools
```

### **3. Create a QueryClient Instance**

Modify or create `src/queryClient.js`:

```javascript
import { QueryClient } from "@tanstack/react-query";

export const queryClient = new QueryClient();
```

### **4. Wrap Your App with `QueryClientProvider`**

Modify `src/index.js` (or `src/main.jsx` in Vite projects):

```javascript
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import { QueryClientProvider } from "@tanstack/react-query";
import { queryClient } from "./queryClient";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <QueryClientProvider client={queryClient}>
    <App />
  </QueryClientProvider>
);
```

### **5. (Optional) Add React Query DevTools**

Modify `src/index.js` (or `src/main.jsx`):

```javascript
import { ReactQueryDevtools } from "@tanstack/react-query-devtools";

<QueryClientProvider client={queryClient}>
  <App />
  <ReactQueryDevtools initialIsOpen={false} />
</QueryClientProvider>;
```

### **6. Use `useQuery` in a Component**

Modify `src/components/Posts.js`:

```javascript
import { useQuery } from "@tanstack/react-query";
import axios from "axios";

const fetchPosts = async () => {
  const { data } = await axios.get(
    "https://jsonplaceholder.typicode.com/posts"
  );
  return data;
};

const Posts = () => {
  const { data, error, isLoading } = useQuery({
    queryKey: ["posts"],
    queryFn: fetchPosts,
  });

  if (isLoading) return <p>Loading...</p>;
  if (error) return <p>Error: {error.message}</p>;

  return (
    <ul>
      {data.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
};

export default Posts;
```

Now React Query is set up and ready to use in your React 18 project! 🚀

```

Now you can copy and paste it directly! 🚀
```
