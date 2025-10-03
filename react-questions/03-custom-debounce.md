# Question 3: Write a custom debounce function and use it

## Problem Statement

Create a custom debounce function. Also, create a function that logs a message to the console, then call it through the debounce.

## Solution

### Steps:

1. Implement debounce function which delays the function call.
2. Use `setTimeout` and `clearTimeout` to delay and cancel calls.
3. Demonstrate debounce by wrapping a logging function.

### Code Example:

```tsx
function debounce<T extends (...args: any[]) => void>(
  func: T,
  delay: number
): T {
  let timeoutId: ReturnType<typeof setTimeout>

  return function (...args: any[]) {
    if (timeoutId) clearTimeout(timeoutId)

    timeoutId = setTimeout(() => {
      func(...args)
    }, delay)
  } as T
}

// Example function to be debounced
function logMessage(message: string) {
  console.log('Logged message:', message)
}

const debouncedLog = debounce(logMessage, 1000)

debouncedLog('Hello')
debouncedLog('Hello again') // Only this will log after 1s delay
```

## Tips

- Useful for preventing function flooding in UI events.
- Always clear timeout before setting a new one.

## Complexity

- Time: O(1) per debounce call.
- Space: O(1).
