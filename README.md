# 📘 `useEvent` Hook Documentation

## ⚡ Quick Start Example

```jsx
import React from "react";
import { useEvent } from "@khmilevoi/use-event"; // adjust the import path accordingly

function MyComponent() {
  const handleClick = useEvent((event) => {
    console.log("Button clicked!", event);
  });

  return <button onClick={handleClick}>Click me</button>;
}

export default MyComponent;
```

---

## 📦 Installation

You can install the package using one of the following package managers:

- **npm**:
  ```bash
  npm install @khmilevoi/use-event
  ```
- **Yarn**:
  ```bash
  yarn add @khmilevoi/use-event
  ```
- **pnpm**:
  ```bash
  pnpm add @khmilevoi/use-event
  ```

---

## 🛠️ Overview

- **Purpose:**  
  Returns a memoized event handler whose reference remains constant across renders while ensuring that the latest callback is executed.

- **Use Case:**  
  Useful for event handlers or callbacks that need to reference updated state or props without causing unnecessary re-renders or re-subscriptions.

- **Reference:**  
  This hook is inspired by the [RFC on useEvent](https://github.com/reactjs/rfcs/blob/useevent/text/0000-useevent.md), which outlines its benefits and design considerations.

---

## 🔍 How It Works

1. **Using a Ref to Store the Callback:**  
   - The hook uses `useRef` to maintain the latest callback.
   - On every render, `callbackRef.current` is updated with the new callback.
  
2. **Stable Callback with `useCallback`:**  
   - The returned function is memoized with an empty dependency array via `useCallback`, ensuring that its identity remains stable.
   - Even though the reference does not change, the function always calls the current callback stored in the ref.

3. **Handling `this` and Arguments:**  
   - The memoized function uses the `apply` method to invoke the current callback with the appropriate `this` context and arguments.

---

## ⚙️ Usage

```jsx
import React from "react";
import { useEvent } from "@khmilevoi/use-event";

function MyComponent() {
  const handleClick = useEvent((event) => {
    console.log("Button clicked!", event);
  });

  return <button onClick={handleClick}>Click me</button>;
}

export default MyComponent;
```

---

## 📑 API Details

| **Property**       | **Description**                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------|
| `callback`         | The event handler function that may change over time but should always execute the latest version. |
| **Return Value**   | A memoized function that is stable across renders but always calls the current version of `callback`. |

---

## 📝 Implementation Details

```tsx
import { useCallback, useRef } from "react";

export function useEvent<T extends Function>(callback: T): T {
  const callbackRef = useRef(callback);

  // Always update the ref to the latest callback
  callbackRef.current = callback;

  // Return a memoized function that always calls the latest callback
  return useCallback(function(this: any, ...args: any[]) {
    return callbackRef.current.apply(this, args);
  }, []) as unknown as T;
}
```

- **Ref Update:**  
  The line `callbackRef.current = callback;` ensures that even if the `callback` changes on subsequent renders, the latest version is always stored in the ref.

- **Memoization:**  
  The `useCallback` hook with an empty dependency array (`[]`) guarantees that the returned function's identity does not change, which can be beneficial for performance and avoiding unnecessary re-renders.

- **Type Casting:**  
  The returned function is cast to `T` to match the type of the input callback, ensuring type safety.

---

## 🔗 Related Information from the RFC

- The [RFC for useEvent](https://github.com/reactjs/rfcs/blob/useevent/text/0000-useevent.md) discusses the challenges with stale closures in React event handlers and proposes this pattern as a solution.
- **Key Insights from the RFC:**
  - **Stale Closure Problem:**  
    Traditional event handlers may capture outdated state or props.
  - **Stable Function Identity:**  
    By keeping a stable reference, components can avoid unnecessary re-subscriptions or re-rendering in child components.
  - **Flexibility:**  
    The pattern allows for callbacks that update over time without affecting the identity of the event handler passed to components.

---

## ✅ Key Benefits

- **Stable Identity:**  
  Ensures that the event handler function remains the same between renders.
  
- **Up-to-Date Callback:**  
  Always calls the latest version of the callback, solving the stale closure issue.
  
- **Performance Optimization:**  
  Avoids unnecessary re-renders and re-attachments of event listeners.

