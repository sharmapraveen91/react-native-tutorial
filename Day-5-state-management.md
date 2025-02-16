# State Management in React Native (with TypeScript)

State management is an important concept in React Native, especially when developing applications that need to handle complex and dynamic data. In this study material, we will explore various state management techniques in React Native using TypeScript, including `useState`, `useReducer`, `Context API`, and popular libraries like `Redux Toolkit` and `Zustand`.

## Table of Contents
1. [useState](#usestate)
2. [useReducer](#usereducer)
3. [Context API](#context-api)
4. [Redux Toolkit](#redux-toolkit)
5. [Zustand](#zustand)
6. [Interview Questions and Answers](#interview-questions-and-answers)

---

## 1. useState

The `useState` hook is the simplest way to manage state in a React Native app. It allows you to store and update state in a functional component.

### Example: Counter with useState

```tsx
import React, { useState } from 'react';
import { View, Text, Button } from 'react-native';

const Counter: React.FC = () => {
  const [count, setCount] = useState<number>(0);

  return (
    <View>
      <Text>Counter: {count}</Text>
      <Button title="Increment" onPress={() => setCount(count + 1)} />
      <Button title="Decrement" onPress={() => setCount(count - 1)} />
    </View>
  );
};

export default Counter;
```
### Explanation:
`useState<number>(0)` initializes the state variable count with an initial value of 0 (with type annotation).
`setCount` is the `function` to update the `state`.
The `Button` components call `setCount` to change the `state`, causing a re-render of the component.
2. `useReducer`
The `useReducer` hook is used for more complex state management, especially when state changes involve multiple sub-values or are more intricate. It’s similar to useState but allows for more control over state updates.

### Example: Counter with useReducer
```tsx
import React, { useReducer } from 'react';
import { View, Text, Button } from 'react-native';

// Define the reducer function
type State = { count: number };
type Action = { type: 'increment' | 'decrement' };

const counterReducer = (state: State, action: Action): State => {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      return state;
  }
};

const Counter: React.FC = () => {
  const [state, dispatch] = useReducer(counterReducer, { count: 0 });

  return (
    <View>
      <Text>Counter: {state.count}</Text>
      <Button title="Increment" onPress={() => dispatch({ type: 'increment' })} />
      <Button title="Decrement" onPress={() => dispatch({ type: 'decrement' })} />
    </View>
  );
};

export default Counter;
```

### Explanation:
The `State` and Action types define the shape of the `state` and `actions` in `TypeScript`.
The `counterReducer` function processes `actions` and `updates` the `state` accordingly.
The `dispatch` function is used to trigger actions.
3. `Context API`
The `React Context API` provides a way to share state across the entire component tree without passing props manually at every level. It’s useful for avoiding "prop drilling" (passing props through many layers).

### Example: Theme Context
```tsx
import React, { useContext, useState } from 'react';
import { View, Text, Button } from 'react-native';

// Create a Context
type ThemeContextType = {
  isDarkMode: boolean;
  setIsDarkMode: (isDark: boolean) => void;
};

const ThemeContext = React.createContext<ThemeContextType | undefined>(undefined);

const useTheme = (): ThemeContextType => {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme must be used within a ThemeProvider');
  }
  return context;
};

const ThemeProvider: React.FC = ({ children }) => {
  const [isDarkMode, setIsDarkMode] = useState<boolean>(false);

  return (
    <ThemeContext.Provider value={{ isDarkMode, setIsDarkMode }}>
      {children}
    </ThemeContext.Provider>
  );
};

const ThemedComponent: React.FC = () => {
  const { isDarkMode, setIsDarkMode } = useTheme();

  return (
    <View style={{ padding: 20 }}>
      <Text style={{ color: isDarkMode ? 'white' : 'black' }}>
        {isDarkMode ? 'Dark Mode' : 'Light Mode'}
      </Text>
      <Button title="Toggle Theme" onPress={() => setIsDarkMode(!isDarkMode)} />
    </View>
  );
};

const App: React.FC = () => (
  <ThemeProvider>
    <ThemedComponent />
  </ThemeProvider>
);

export default App;
```

### Explanation:
The `ThemeContext` is typed using `ThemeContextType` to ensure type safety.
`useTheme` is a custom hook to access the theme context.
`ThemeProvider` provides the context to the rest of the app.
4. `Redux Toolkit`
`Redux` is a predictable state container for JavaScript apps, and `Redux` Toolkit is the official, recommended way to write Redux logic. It simplifies many common patterns in Redux, like action creators and reducers.

Example: Counter with Redux Toolkit
First, install Redux Toolkit and React-Redux with Yarn:
```bash
yarn add @reduxjs/toolkit react-redux
```

## 4.1 Setting up Redux Store

```tsx
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from './counterSlice';

const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
});

export default store;
```

## 4.2 Creating a Slice
```tsx
import { createSlice, PayloadAction } from '@reduxjs/toolkit';

interface CounterState {
  count: number;
}

const initialState: CounterState = { count: 0 };

const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment(state) {
      state.count += 1;
    },
    decrement(state) {
      state.count -= 1;
    },
  },
});

export const { increment, decrement } = counterSlice.actions;
export default counterSlice.reducer;
```

## 4.3 Connecting Redux to React Native

```tsx
import React from 'react';
import { View, Text, Button } from 'react-native';
import { useDispatch, useSelector } from 'react-redux';
import { increment, decrement } from './counterSlice';

const Counter: React.FC = () => {
  const count = useSelector((state: { counter: CounterState }) => state.counter.count);
  const dispatch = useDispatch();

  return (
    <View>
      <Text>Counter: {count}</Text>
      <Button title="Increment" onPress={() => dispatch(increment())} />
      <Button title="Decrement" onPress={() => dispatch(decrement())} />
    </View>
  );
};

export default Counter;
```

## 4.4 Providing Redux Store

```tsx
import React from 'react';
import { Provider } from 'react-redux';
import { View } from 'react-native';
import store from './store';
import Counter from './Counter';

const App: React.FC = () => (
  <Provider store={store}>
    <Counter />
  </Provider>
);

export default App;
```

### Explanation:
`Slice`: We use `createSlice` to define actions (increment, decrement) and `reducers` in a concise way.
`Store`: The configureStore function sets up the Redux store.
Provider: The `Provider` component from `react-redux` makes the store available to the app.
`useSelector`: This `hook` is used to access the Redux state.
`useDispatch`: This hook dispatches actions to modify the Redux state.
## 5. Zustand
`Zustand` is a minimalistic state management library that works well with `React` and` React Nativ`e. It’s simple and provides a flexible API for state management.

### Example: Counter with Zustand
First, install Zustand with Yarn:
```bash
yarn add zustand
```

## 5.1 Creating a Store

```tsx
import create from 'zustand';

interface CounterState {
  count: number;
  increment: () => void;
  decrement: () => void;
}

const useStore = create<CounterState>((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
  decrement: () => set((state) => ({ count: state.count - 1 })),
}));

5.2 Using Zustand in a Component

import React from 'react';
import { View, Text, Button } from 'react-native';
import useStore from './store';

const Counter: React.FC = () => {
  const { count, increment, decrement } = useStore();

  return (
    <View>
      <Text>Counter: {count}</Text>
      <Button title="Increment" onPress={increment} />
      <Button title="Decrement" onPress={decrement} />
    </View>
  );
};

export default Counter;
```

## Zustand

Explanation:
- `create` is used to define the store and state actions (increment, decrement).
- `useStore` is the custom hook that provides access to the state and actions.
- Zustand allows a very straightforward, minimalistic approach to managing global state.

---


## Interview Questions and Answers

Here are 20 interview questions related to state management in React Native, along with detailed answers:

### 1. What is state in React Native?
   **Answer**: State refers to the data or variables in a React component that can change over time and affect the component's output. React Native uses state to control how a component behaves and displays content.

### 2. What is the difference between `useState` and `useReducer` in React?
   **Answer**: `useState` is simpler and is used for local component state. `useReducer` is used when the state logic is complex, such as when actions affect multiple properties of the state. `useReducer` can provide more control and is typically used for managing more complex state updates.

### 3. When should you use Context API in React Native?
   **Answer**: You should use Context API when you need to share data across many components at different nesting levels without having to pass props down manually through each level (i.e., avoiding "prop drilling").

### 4. What is Redux, and how does it help with state management?
   **Answer**: Redux is a state management library that provides a global store for your application. It allows you to centralize the state and use actions and reducers to update the state. It helps in managing global state and debugging complex state transitions.

### 5. What is the purpose of the `Provider` component in Redux?
   **Answer**: The `Provider` component makes the Redux store available to all components in the app. It ensures that every component can access the store using `useSelector` and dispatch actions with `useDispatch`.

### 6. What are the key features of Redux Toolkit?
   **Answer**: Redux Toolkit simplifies Redux by providing utilities like `createSlice` to automatically generate reducers and actions, `configureStore` for setting up the Redux store, and devtools integration for easy debugging.

### 7. How does `useSelector` work in Redux?
   **Answer**: `useSelector` is a hook that allows a component to access the state in the Redux store. It subscribes to the Redux store and re-renders the component whenever the selected state changes.

### 8. What is `useDispatch` used for in Redux?
   **Answer**: `useDispatch` is a hook that gives you access to the Redux store's dispatch function, allowing you to dispatch actions to update the state.

### 9. What is the main advantage of using Zustand for state management?
   **Answer**: Zustand is a minimalistic, lightweight state management solution that offers an easy-to-use API, avoids boilerplate code, and is ideal for applications with simple state management needs.

### 10. Can Zustand be used with Redux?
   **Answer**: Yes, Zustand can coexist with Redux. However, they serve different purposes. Zustand is a lightweight alternative to Redux, while Redux is suitable for managing large, complex, and global states.

### 11. How do you handle async actions in Redux?
   **Answer**: Async actions in Redux are handled using middleware like `redux-thunk` or `redux-saga`. These libraries allow you to dispatch actions that return a function or promise, enabling you to manage side effects such as data fetching.

### 12. Explain the concept of reducers in Redux.
   **Answer**: Reducers are pure functions that define how the state changes in response to actions. They take the current state and an action, and return a new state.

### 13. What is the role of actions in Redux?
   **Answer**: Actions in Redux are plain JavaScript objects that describe the type of change you want to make to the state. Actions are dispatched to the store and processed by reducers to update the state.

### 14. What is a slice in Redux Toolkit?
   **Answer**: A slice is a part of the Redux state and the corresponding reducers and actions that operate on that part of the state. `createSlice` from Redux Toolkit automatically generates the action creators and reducers for a specific piece of state.

### 15. How would you handle global state in a large React Native app?
   **Answer**: In a large app, you can handle global state using Redux, Context API, or Zustand, depending on the app's complexity. For complex state, Redux is often a preferred choice, while for simpler state, Context API or Zustand could suffice.

### 16. How do you optimize performance when using Context API?
   **Answer**: To optimize performance with Context API, you can split contexts into smaller, focused contexts, and avoid passing unnecessary state to the components. Using `React.memo` or `useMemo` can also help reduce unnecessary re-renders.

### 17. What are side effects in React, and how do you handle them?
   **Answer**: Side effects are operations like data fetching, subscriptions, or manual DOM manipulation that happen outside the component's rendering process. In React, side effects are handled using `useEffect`.

### 18. Explain the difference between local component state and global state.
   **Answer**: Local component state is used within a single component (like `useState`), while global state is shared across multiple components (like in Redux or Context API). Global state is used for managing data that needs to be accessed or modified by different parts of the app.

### 19. What is the `useEffect` hook, and how does it relate to state management?
   **Answer**: `useEffect` is a hook used to handle side effects in React. It can be used to fetch data or perform operations when a component mounts or updates, and it can also be used to update the state after performing async operations.

### 20. How would you debug Redux state?
   **Answer**: Redux state can be debugged using Redux DevTools, which allows you to inspect actions and the state tree. It also allows you to time-travel through dispatched actions and observe state changes over time.
