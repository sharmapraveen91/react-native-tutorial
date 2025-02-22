# Redux State Management in React Native

## Introduction
State management in React Native is like managing a library. Imagine each component in your app as a librarian who needs books (data). Without a proper system, librarians would constantly ask each other for books, causing confusion. **Redux** acts like a central library catalog—every librarian can check it to see where books are, ensuring smooth and efficient management.

---

## 1. Why Use Redux?
- **Global State Management:** Access data from anywhere in your app.
- **Predictability:** State updates follow strict rules, making behavior predictable.
- **Debugging:** Redux DevTools help inspect and track state changes.

---

## 2. How Redux Works
Think of Redux as having three main parts:
1. **Store:** The central library that holds all the books (state).
2. **Actions:** Requests made by librarians to get or update books.
3. **Reducers:** The librarian in charge of updating the library based on requests.

---

## 3. Step-by-Step Example: Redux in React Native

### 3.1 Installing Redux
```bash
yarn add @reduxjs/toolkit react-redux
```

---

### 3.2 Setting Up Redux Store
```typescript
// store.ts
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from './counterSlice';

const store = configureStore({
    reducer: {
        counter: counterReducer,
    },
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
export default store;
```

---

### 3.3 Creating a Redux Slice
```typescript
// counterSlice.ts
import { createSlice, PayloadAction } from '@reduxjs/toolkit';

interface CounterState {
    value: number;
}

const initialState: CounterState = { value: 0 };

const counterSlice = createSlice({
    name: 'counter',
    initialState,
    reducers: {
        increment: (state) => {
            state.value += 1;
        },
        decrement: (state) => {
            state.value -= 1;
        },
        incrementByAmount: (state, action: PayloadAction<number>) => {
            state.value += action.payload;
        },
    },
});

export const { increment, decrement, incrementByAmount } = counterSlice.actions;
export default counterSlice.reducer;
```

---

### 3.4 Providing the Store to the App
```typescript
// App.tsx
import React from 'react';
import { Provider } from 'react-redux';
import store from './store';
import CounterScreen from './CounterScreen';

const App = () => {
    return (
        <Provider store={store}>
            <CounterScreen />
        </Provider>
    );
};

export default App;
```

---

### 3.5 Using Redux State and Dispatch
```typescript
// CounterScreen.tsx
import React from 'react';
import { View, Text, Button } from 'react-native';
import { useSelector, useDispatch } from 'react-redux';
import { RootState } from './store';
import { increment, decrement, incrementByAmount } from './counterSlice';

const CounterScreen = () => {
    const count = useSelector((state: RootState) => state.counter.value);
    const dispatch = useDispatch();

    return (
        <View>
            <Text>Count: {count}</Text>
            <Button title="Increment" onPress={() => dispatch(increment())} />
            <Button title="Decrement" onPress={() => dispatch(decrement())} />
            <Button title="Increment by 5" onPress={() => dispatch(incrementByAmount(5))} />
        </View>
    );
};

export default CounterScreen;
```

---

## 4. Do's and Don'ts

✅ **Do's:**
- Use Redux for **global state** that needs to be shared across components.
- Organize code into **slices** for maintainability.
- Use **useSelector** and **useDispatch** hooks for cleaner code.

❌ **Don'ts:**
- Don’t use Redux for **local component state** (use `useState` instead).
- Avoid **overusing** Redux—it can add unnecessary complexity.
- Don’t **mutate state** directly in reducers.

---

## 5. Best Practices
- Use **Redux Toolkit** for cleaner and more concise code.
- Use **TypeScript** for type safety.
- Use **Redux DevTools** for debugging.
- Keep **reducers pure**—no side effects inside them.
- Structure the app using **feature-based folders**.

---

## 6. Use Cases

### ✅ When to Use Redux
- Sharing state across multiple components
- Managing complex state logic
- Tracking app-wide settings (e.g., themes, user preferences)

### ❌ When Not to Use Redux
- Simple local state within a single component
- Temporary UI state (e.g., form inputs, toggles)
- State that doesn’t need to persist outside a component

---

## 7. Interview Questions and Answers

1. **What is Redux?**  
Redux is a state management library that provides a predictable state container for JavaScript applications.

2. **What are the main components of Redux?**  
Store, Actions, and Reducers.

3. **What is Redux Toolkit?**  
Redux Toolkit is an official, opinionated, and recommended way to write Redux code, making it easier and less verbose.

4. **Why should you use Redux Toolkit?**  
It simplifies Redux code, reduces boilerplate, and includes tools like `createSlice` and `configureStore`.

5. **What is an Action in Redux?**  
An Action is a plain object that describes what needs to be done.

6. **What is a Reducer in Redux?**  
A Reducer is a pure function that updates the state based on the action received.

7. **What is the Store in Redux?**  
The Store is a central repository that holds the entire state of the application.

8. **What is `useSelector` used for?**  
`useSelector` is a hook that allows components to read data from the Redux store.

9. **What is `useDispatch` used for?**  
`useDispatch` is a hook that allows components to dispatch actions to the Redux store.

10. **How does Redux DevTools help?**  
Redux DevTools allows you to inspect, debug, and track state changes in your app.

11. **What is the difference between `useState` and Redux?**  
`useState` is for local component state, while Redux is for global state shared across components.

12. **Why should reducers be pure functions?**  
Reducers must be pure to ensure predictable state updates without side effects.

13. **What is the purpose of `configureStore` in Redux Toolkit?**  
`configureStore` simplifies store setup and provides default middleware.

14. **Can you have multiple stores in Redux?**  
While possible, it is not recommended. Use a single store for the entire app.

15. **How do you handle asynchronous logic in Redux?**  
Using middleware like Redux Thunk or Redux Saga.

16. **What happens if a reducer returns `undefined`?**  
Redux throws an error because reducers must always return a valid state.

17. **Why should you not mutate state in reducers?**  
Mutating state breaks Redux's predictability and makes debugging harder.

18. **What is the difference between `createSlice` and traditional Redux?**  
`createSlice` reduces boilerplate by combining actions and reducers into a single file.

19. **How do you initialize the Redux store with default values?**  
By passing an `initialState` to each slice or reducer.

20. **How can you improve Redux performance?**  
Avoid unnecessary re-renders with `useSelector`, split the store into slices, and keep reducers simple.

---

## Conclusion
Redux is a powerful state management tool that helps maintain predictable and consistent app behavior. Using **Redux Toolkit** simplifies setup and reduces boilerplate, making it perfect for both beginners and experienced developers.
