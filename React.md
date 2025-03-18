# React Interview Questions and Answers

## 📌 Basic Level

### 1. What is React?
**Answer:** React is a JavaScript library for building user interfaces, developed by Facebook. It allows developers to create reusable UI components and manage application state efficiently using a virtual DOM.

### 2. What are the key features of React?
**Answer:**
- **JSX (JavaScript XML):** A syntax extension for JavaScript to write HTML in React.
- **Components:** Reusable building blocks of a UI.
- **Virtual DOM:** Efficiently updates the UI by minimizing real DOM manipulations.
- **Unidirectional Data Flow:** Data flows in a single direction, making debugging easier.
- **Hooks:** Allow functional components to use state and lifecycle features.

### 3. What is the difference between functional and class components?
**Answer:**
- **Functional Components:** Use functions, are stateless (prior to hooks), and are simpler.
- **Class Components:** Use ES6 classes, can have state and lifecycle methods.

### 4. What is the Virtual DOM?
**Answer:** The Virtual DOM is a lightweight copy of the actual DOM. React updates the Virtual DOM first, compares it with the previous version (diffing), and then updates only the necessary parts of the real DOM, improving performance.

### 5. What are props in React?
**Answer:** Props (short for properties) are used to pass data from a parent component to a child component.

### 6. What is state in React?
**Answer:** State is an object that holds dynamic data that can change over time and influence the rendering of a component.

### 7. What is React Fragment?
**Answer:** A React Fragment is a lightweight wrapper used to group multiple elements without adding extra nodes to the DOM.
```jsx
<>
  <h1>Hello</h1>
  <p>World</p>
</>
```

### 8. What are controlled and uncontrolled components?
**Answer:**
- **Controlled Components:** Components where form elements are controlled by React state.
- **Uncontrolled Components:** Components where form elements manage their own state via the DOM.

### 9. What is the use of keys in React?
**Answer:** Keys help React identify which elements have changed, are added, or removed, improving the efficiency of rendering lists.

### 10. What is the difference between React and ReactDOM?
**Answer:**
- **React:** Provides the core functionality, including components and hooks.
- **ReactDOM:** Manages rendering to the DOM.

## 📌 Intermediate Level

### 11. What are React Hooks?
**Answer:** Hooks are functions that allow functional components to use state and lifecycle methods. Examples:
- `useState` – Manages state in functional components.
- `useEffect` – Handles side effects.
- `useContext` – Accesses the React context.
- `useRef` – References DOM elements.
- `useReducer` – Manages complex state logic.

### 12. Explain the useEffect hook with examples.
**Answer:** `useEffect` is a hook for handling side effects like API calls or subscriptions. It runs after render and can be controlled using dependency arrays.
```jsx
useEffect(() => {
  console.log("Component mounted");
  return () => console.log("Component unmounted"); // Cleanup function
}, []);
```

### 13. What is Context API?
**Answer:** The Context API provides a way to pass data through the component tree without prop drilling.
```jsx
const MyContext = React.createContext();

const App = () => {
  return (
    <MyContext.Provider value={"Hello World"}>
      <ChildComponent />
    </MyContext.Provider>
  );
};
```

### 14. What is Redux?
**Answer:** Redux is a state management library for managing global state in large applications.

### 15. What is the difference between useState and useReducer?
**Answer:**
- `useState` is simpler for local state management.
- `useReducer` is better for complex state logic involving multiple sub-values.

### 16. What is the significance of PureComponent?
**Answer:** A PureComponent prevents unnecessary renders by performing a shallow comparison of props and state.

### 17. How does React handle forms?
**Answer:** Forms in React can be controlled (using state) or uncontrolled (using refs).

### 18. What is the difference between React Router and traditional routing?
**Answer:** React Router enables client-side navigation without reloading the page.

### 19. What are error boundaries?
**Answer:** Components that catch JavaScript errors and display fallback UI instead of crashing the whole application.

### 20. How does lazy loading work in React?
**Answer:** React’s `React.lazy()` allows dynamic import of components to improve performance.

## 📌 Advanced Level

### 21. What is React Fiber?
**Answer:** React Fiber is the new reconciliation engine in React 16+. It improves UI rendering performance by breaking work into small units and prioritizing updates.

### 22. What is Server-Side Rendering (SSR)?
**Answer:** SSR renders React components on the server, sending a fully-rendered page to the client, improving SEO and performance. Frameworks like Next.js support SSR.

### 23. What are Higher-Order Components (HOC)?
**Answer:** HOCs are functions that take a component and return a new enhanced component.
```jsx
const withLogger = (Component) => (props) => {
  console.log("Logging props", props);
  return <Component {...props} />;
};
```

### 24. How does React handle memory leaks?
**Answer:** Using cleanup functions in `useEffect`, unmounting components properly, and avoiding unnecessary re-renders help manage memory efficiently.

### 25. What is the Suspense component?
**Answer:** Suspense is used to handle lazy-loaded components and data fetching.

---

## 📌 Coding Questions

### 26. Implement a Counter using Hooks
```jsx
import React, { useState } from 'react';

const Counter = () => {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
};

export default Counter;
```

### 27. Fetch API Data using useEffect
```jsx
import React, { useEffect, useState } from 'react';

const FetchData = () => {
  const [data, setData] = useState([]);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/posts")
      .then(response => response.json())
      .then(json => setData(json));
  }, []);

  return (
    <div>
      {data.map(item => <p key={item.id}>{item.title}</p>)}
    </div>
  );
};

export default FetchData;
```
