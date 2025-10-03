# Question 4: Create a custom countdown timer from 10 to 0

## Problem Statement

Create a React component that acts as a countdown timer, counting down from 10 to 0.

## Solution

### Steps:

1. Use `useState` for the current timer value.
2. Use `useEffect` with `setInterval` to decrease the timer every second.
3. Clear interval on reaching 0.
4. Display the countdown.

### Code Example:

```tsx
import React, { useState, useEffect } from 'react'

const CountdownTimer: React.FC = () => {
  const [count, setCount] = useState(10)

  useEffect(() => {
    if (count === 0) return

    const interval = setInterval(() => {
      setCount((prev) => prev - 1)
    }, 1000)

    return () => clearInterval(interval)
  }, [count])

  return <div>Countdown: {count}</div>
}

export default CountdownTimer
```

## Interviewer Tips

- Always clean up timers to avoid memory leaks.
- Handle edge cases like count reaching zero.

## Complexity

- Time: O(1) per tick.
- Space: O(1).
