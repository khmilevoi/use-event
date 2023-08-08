# React Hook useEvent

## Overview
This repository includes a custom React Hook `useEvent` which is used for handling and passing events. This hook receives a callback function as an argument and returns a memoized version of that callback.

The `useEvent` hook takes advantage of `useRef` and `useCallback` to maintain callback reference and prevent unnecessary re-renders, resulting in an improvement of performance.

## Installation

```sh
npm i @khmilevoi/use-event
```
Then, copy the `useEvent` function to your project.

## Usage
Here's an example of how to use the `useEvent` hook:

```jsx
import { useEvent } from "@khmilevoi/use-event";

// Define your callback
const yourCallback = (arg1, arg2) => {
    // Your callback implementation
};

// Use the useEvent hook
const memoizedCallback = useEvent(yourCallback);
```

In this case, `memoizedCallback` is a memoized version of `yourCallback` that will only change if `yourCallback` does. This is useful when passing callbacks to optimized child components that rely on reference equality to prevent unnecessary renders (e.g., shouldComponentUpdate).

## API
### useEvent(callback)
##### Input
- `callback: Function`: The callback function to be memoized.
##### Output
- Returns a memoized version of the callback function that only changes if the callback does.
## Note
Please note that this custom Hook is not officially a part of the React library. It's a utility function designed to help in certain use cases. Please use it responsibly and with understanding of its implications.

## License
MIT
