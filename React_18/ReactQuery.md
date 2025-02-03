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

## **Auto Refresh**

If the data is staled, React Query will try to fetch the new data from the backend while at the same time returning the stale data from the cache to application. When the new data is fetched, react query provide the fresh data to the component and the component is rerendered if required.
React Query refetches data in the following scenarios:

1. **On Window Focus** – Automatically refetches when the window is refocused.
2. **On Network Status Change** – Refetches when the app goes online after being offline.
3. **Stale Time Expiry** – Refetches when the data becomes stale (default 0ms).
4. **Manual Refetch** – You can call `refetch()` to trigger a refetch.
5. **On Mount** – Refetches the first time a component mounts.
6. **Query Parameters Change** – Refetches when query keys or parameters change.
7. **Polling** – Refetches at regular intervals with `refetchInterval`.

## **Quries**

To subscribe to a query in your components or custom hooks, call the `useQuery` hook with at least:

- A unique key for the query
- A function that returns a promise that:
  - Resolves the data, or
  - Throws an error

```javascript
import { useQuery } from "@tanstack/react-query";

function App() {
  const info = useQuery({ queryKey: ["todos"], queryFn: fetchTodoList });
}
```

## **Query Key in React Query**

A **Query Key** is a unique identifier for each query in React Query. It allows React Query to cache, track, and manage the state of different queries. The key helps React Query to differentiate between queries and refetch or update the data as needed.

- **Simple Query Key**: A string (e.g., `"posts"`) to identify a query.
- **Dynamic Query Key**: An array of values (e.g., `["posts", userId]`) to pass dynamic parameters like filters or pagination.

**If your query function depends on a variable, include it in your query key.**

**Example**:

```javascript
function Todos({ todoId }) {
  const result = useQuery({
    queryKey: ["todos", todoId],
    queryFn: () => fetchTodoById(todoId),
  });
}
```

Note that query keys act as dependencies for your query functions. Adding dependent variables to your query key will ensure that queries are cached independently, and that any time a variable changes, queries will be refetched automatically
Query keys must be unique to ensure correct caching and refetching behavior. React Query uses the key to track each query's data and state.

## **Query Functions**

A query function can be literally any function that returns a promise. The promise that is returned should either resolve the data or throw an error.

All of the following are valid query function configurations:

```javascript
useQuery({ queryKey: ["todos"], queryFn: fetchAllTodos });
useQuery({ queryKey: ["todos", todoId], queryFn: () => fetchTodoById(todoId) });
useQuery({
  queryKey: ["todos", todoId],
  queryFn: async () => {
    const data = await fetchTodoById(todoId);
    return data;
  },
});
useQuery({
  queryKey: ["todos", todoId],
  queryFn: ({ queryKey }) => fetchTodoById(queryKey[1]),
});
```

## **Network Modes**

Here are the different **network modes** in React Query:

### **1. Online Mode (Default)**

- **What it does**: React Query fetches data normally when the network is available.
- **When it's used**: Fetches data when a query is triggered and automatically updates in the background.

### **2. Offline Mode**

- **What it does**: Allows the app to work even without a network connection.
- **When it's used**: Caches the data while offline and syncs it back to the server when the network is restored.

### **3. Background Fetching (Polling)**

- **What it does**: React Query can automatically fetch data at regular intervals.
- **When it's used**: Keeps the data fresh without needing the user to refresh. You can set the interval to update data periodically.

```javascript
useQuery("data", fetchData, { refetchInterval: 60000 }); // Fetch every 60 seconds
```

### **4. Focus Refetching (Auto Refetch)**

- **What it does**: Automatically refetches data when you come back to the app after switching tabs or windows.
- **When it's used**: Ensures that you see the most up-to-date data when returning to the app.

React Query helps manage network behavior to keep data updated and user-friendly.

## **Parallel Queries**

"Parallel" queries are queries that are executed in parallel, or at the same time so as to maximize fetching concurrency.
When the number of parallel queries does not change, there is no extra effort to use parallel queries. Just use any number of TanStack Query's `useQuery` and `useInfiniteQuery` hooks side-by-side!

```javascript
function App () {
  // The following queries will execute in parallel
  const usersQuery = useQuery({ queryKey: ['users'], queryFn: fetchUsers })
  const teamsQuery = useQuery({ queryKey: ['teams'], queryFn: fetchTeams })
  const projectsQuery = useQuery({ queryKey: ['projects'], queryFn: fetchProjects })
  ...
}
```

## **Dependent Queries**

Dependent (or serial) queries depend on previous ones to finish before they can execute. To achieve this, it's as easy as using the `enabled` option to tell a query when it is ready to run:

```javascript
// Get the user
const { data: user } = useQuery({
  queryKey: ["user", email],
  queryFn: getUserByEmail,
});

const userId = user?.id;

// Then get the user's projects
const {
  status,
  fetchStatus,
  data: projects,
} = useQuery({
  queryKey: ["projects", userId],
  queryFn: getProjectsByUser,
  // The query will not execute until the userId exists
  enabled: !!userId,
});
```

## **Paginated / Lagged Queries**

Rendering paginated data is a very common UI pattern and in TanStack Query, it "just works" by including the page information in the query key:

```javascript
const result = useQuery({
  queryKey: ["projects", page],
  queryFn: fetchProjects,
});
```

### **Better Paginated Queries with `placeholderData`**

Consider the following example where we would ideally want to increment a `pageIndex` (or cursor) for a query. If we were to use `useQuery`, it would still technically work fine, but the UI would jump in and out of the success and pending states as different queries are created and destroyed for each page or cursor.

By setting `placeholderData` to `(previousData) => previousData` or using the `keepPreviousData` function exported from TanStack Query, we get a few new benefits:

- The data from the last successful fetch is available while new data is being requested, even though the query key has changed.
- When the new data arrives, the previous data is seamlessly swapped to show the new data.
- `isPlaceholderData` is made available to know what data the query is currently providing you.
