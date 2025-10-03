# Question 2: Create a custom hook to resize the window and show height and width

## Problem Statement

Create a custom React hook that tracks the browser window size, updating when the window is resized. Display the current height and width using this hook.

## Solution

### Steps:

1. Use `useState` to store window height and width.
2. Use `useEffect` to add an event listener on `resize` event.
3. Update state inside the event listener.
4. Cleanup the event listener on unmount.
5. Return the window size from the hook and consume it in a component.

### Code Example:

```tsx
import { useState, useEffect } from 'react'

function useWindowSize() {
  const [windowSize, setWindowSize] = useState({
    width: window.innerWidth,
    height: window.innerHeight,
  })

  useEffect(() => {
    function handleResize() {
      setWindowSize({ width: window.innerWidth, height: window.innerHeight })
    }

    window.addEventListener('resize', handleResize)
    return () => window.removeEventListener('resize', handleResize)
  }, [])

  return windowSize
}

export default useWindowSize
```

### Usage:

```tsx
import React from 'react'
import useWindowSize from './useWindowSize'

const WindowSizeComponent = () => {
  const { width, height } = useWindowSize()

  return (
    <div>
      <p>Width: {width}</p>
      <p>Height: {height}</p>
    </div>
  )
}

export default WindowSizeComponent
```

## Interviewer Tips

- Remember to clean up event listeners to avoid leaks.
- Initialize state carefully to avoid issues in SSR.

## Complexity

- Time: O(1) per resize event.
- Space: O(1).
