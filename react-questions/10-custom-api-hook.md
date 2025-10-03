# Question 10: Create a custom hook for API call which accepts base URL

## Problem Statement

Create a reusable React hook that accepts a base URL and returns fetched data, loading state, and error.

## Solution

### Steps:

1. Accept base URL param.
2. Use `useEffect` to fetch data.
3. Return data, loading, and error.
4. Generic typing to handle various data.

### Code Example:

```tsx
import { useState, useEffect } from 'react'

function useApi<T>(baseUrl: string) {
  const [data, setData] = useState<T | null>(null)
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState<Error | null>(null)

  useEffect(() => {
    setLoading(true)
    fetch(baseUrl)
      .then((res) => {
        if (!res.ok) throw new Error('Network response not ok')
        return res.json()
      })
      .then(setData)
      .catch(setError)
      .finally(() => setLoading(false))
  }, [baseUrl])

  return { data, loading, error }
}

export default useApi
```

## Interviewer Tips

- Generic hook supports multiple data types.
- Handle errors and loading properly.

## Complexity

- Time and space depend on fetched data.
