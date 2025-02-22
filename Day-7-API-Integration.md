# API Integration & Data Handling in React Native

## Introduction
API integration is essential for mobile apps to interact with servers and fetch data. In React Native, you can use tools like **Fetch API**, **Axios**, and **RTK Query** to make these interactions smooth and efficient.

Think of an API as a waiter in a restaurant. You (the app) request food (data), the waiter (API) delivers your request to the kitchen (server), and then brings back the food (response). The right tools make this process quick and accurate.

---

## 1. Fetch API

**Fetch API** is built into JavaScript, making it lightweight and easy to use.

### Example:
```typescript
const fetchData = async () => {
    try {
        const response = await fetch('https://jsonplaceholder.typicode.com/posts');
        if (!response.ok) throw new Error('Network response was not ok');
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error('Error fetching data:', error);
    }
};
```

### Analogy:
- Using **fetch** is like calling a restaurant to order food.

### Do's:
- ✅ Use `async/await` for readability.
- ✅ Always handle errors using `try/catch`.
- ✅ Check `response.ok` before parsing data.

### Don'ts:
- ❌ Don't forget to handle network errors.
- ❌ Don't parse the response without checking if it's OK.

---

## 2. Axios

**Axios** is a popular library that simplifies API calls with additional features like automatic JSON parsing, request cancellation, and interceptors.

### Installation:
```bash
yarn add axios
```

### Example:
```typescript
import axios from 'axios';

const fetchData = async () => {
    try {
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts');
        console.log(response.data);
    } catch (error) {
        console.error('Error fetching data:', error);
    }
};
```

### Analogy:
- Using **Axios** is like using a food delivery app (faster, easier, with more features).

### Do's:
- ✅ Use Axios for advanced use cases.
- ✅ Handle errors using `.catch()` or `try/catch`.
- ✅ Configure Axios with base URLs and interceptors.

### Don'ts:
- ❌ Don't forget to cancel requests if the component unmounts.
- ❌ Don't ignore errors; always provide user feedback.

---

## 3. RTK Query

**RTK Query** is part of Redux Toolkit, designed for efficient and simplified data fetching.

### Installation:
```bash
yarn add @reduxjs/toolkit react-redux
```

### Setup:
```typescript
// store.ts
import { configureStore } from '@reduxjs/toolkit';
import { postsApi } from './services/postsApi';

export const store = configureStore({
    reducer: {
        [postsApi.reducerPath]: postsApi.reducer,
    },
    middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(postsApi.middleware),
});
```

### Service:
```typescript
// services/postsApi.ts
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react';

export const postsApi = createApi({
    reducerPath: 'postsApi',
    baseQuery: fetchBaseQuery({ baseUrl: 'https://jsonplaceholder.typicode.com/' }),
    endpoints: (builder) => ({
        getPosts: builder.query({ query: () => 'posts' }),
    }),
});

export const { useGetPostsQuery } = postsApi;
```

### Usage:
```typescript
// App.tsx
import React from 'react';
import { useGetPostsQuery } from './services/postsApi';

const App = () => {
    const { data, error, isLoading } = useGetPostsQuery();

    if (isLoading) return <Text>Loading...</Text>;
    if (error) return <Text>Error loading data</Text>;

    return (
        <View>
            {data?.map((post) => (
                <Text key={post.id}>{post.title}</Text>
            ))}
        </View>
    );
};

export default App;
```

### Analogy:
- Using **RTK Query** is like having a robot waiter who remembers your orders and delivers faster.

### Do's:
- ✅ Use RTK Query for scalable apps.
- ✅ Handle loading and error states.
- ✅ Use caching to optimize performance.

### Don'ts:
- ❌ Don't manually manage state if using RTK Query.
- ❌ Avoid redundant API calls.

---

## Best Practices
- 🟢 Use environment variables for API URLs.
- 🟢 Use `.env` files to manage secrets.
- 🟢 Always validate API responses.
- 🟢 Cache data whenever possible.
- 🟢 Follow SOLID principles and Clean Code.

---

## Interview Questions and Answers

### Basic Level
1. **What is an API?**
   - An API (Application Programming Interface) allows different systems to communicate with each other.

2. **What is the Fetch API?**
   - A built-in JavaScript API for making network requests.

3. **How do you handle errors with Fetch?**
   - Use `try/catch` and check `response.ok`.

4. **What is Axios?**
   - A promise-based HTTP client for browsers and Node.js.

5. **Why use Axios instead of Fetch?**
   - Axios is easier to use, has automatic JSON parsing, and supports request cancellation.

### Intermediate Level
6. **What is RTK Query?**
   - A data-fetching library in Redux Toolkit that simplifies API calls and caching.

7. **How do you define an API endpoint in RTK Query?**
   - Using `createApi()` and `builder.query()` or `builder.mutation()`.

8. **How does RTK Query handle caching?**
   - It caches data by default to reduce API calls.

9. **What is `baseQuery` in RTK Query?**
   - A function that handles the API call, typically using `fetchBaseQuery()`.

10. **How do you cancel an API request in Axios?**
   - Use `axios.CancelToken.source()`.

### Advanced Level
11. **Explain the difference between Fetch, Axios, and RTK Query.**
    - Fetch is built-in, Axios is feature-rich, and RTK Query integrates with Redux.

12. **How do you manage global API state using RTK Query?**
    - Using `createApi()` and configuring the Redux store.

13. **What are Axios interceptors?**
    - Functions that intercept requests and responses to add custom logic.

14. **How does RTK Query optimize performance?**
    - By caching data and reducing unnecessary API calls.

15. **How do you secure API keys in React Native?**
    - Use environment variables and never hardcode keys.

### Scenario-Based Questions
16. **What would you do if an API call is slow?**
    - Optimize the API, cache responses, or use pagination.

17. **How do you handle 404 and 500 errors?**
    - Display user-friendly error messages and log details.

18. **How would you refresh stale API data in RTK Query?**
    - Use `refetch()` or configure cache settings.

19. **How do you handle pagination with Fetch or Axios?**
    - Append query parameters like `?page=1`.

20. **How do you manage API timeouts in Axios?**
    - Set a `timeout` in Axios configuration.

---

## Conclusion
Using Fetch API, Axios, and RTK Query helps you integrate APIs in React Native apps efficiently. Choose the right tool based on your app’s needs, follow best practices, and always handle errors to ensure a smooth user experience.

---

Feel free to modify this material as needed!
