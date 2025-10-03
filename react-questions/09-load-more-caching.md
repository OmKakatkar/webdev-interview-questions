# Question 9: Implement Load More functionality and caching in React

## Problem Statement

Implement a "Load More" to fetch additional pages of data and cache previously loaded data for faster access.

## Solution

### Steps:

1. Maintain state for all loaded items and current page.
2. Fetch data in pages.
3. Append new data on Load More.
4. Cache all loaded data in state for render.

### Code Example:

```tsx
import React, { useState, useEffect } from 'react'

const PAGE_SIZE = 10

const LoadMoreWithCache: React.FC = () => {
  const [items, setItems] = useState<any[]>([])
  const [page, setPage] = useState(1)
  const [loading, setLoading] = useState(false)

  useEffect(() => {
    loadData(page)
  }, [page])

  const loadData = async (pageNum: number) => {
    setLoading(true)
    const res = await fetch(
      `https://jsonplaceholder.typicode.com/posts?_page=${pageNum}&_limit=${PAGE_SIZE}`
    )
    const data = await res.json()
    setItems((prev) => [...prev, ...data])
    setLoading(false)
  }

  const handleLoadMore = () => {
    setPage((prev) => prev + 1)
  }

  return (
    <div>
      <ul>
        {items.map((item) => (
          <li key={item.id}>{item.title}</li>
        ))}
      </ul>
      <button
        onClick={handleLoadMore}
        disabled={loading}>
        Load More
      </button>
    </div>
  )
}

export default LoadMoreWithCache
```

## Interviewer Tips

- Explain caching to avoid refetching.
- Handle loading states to prevent multiple requests.

## Complexity

- Time: O(pages \* pageSize).
- Space: O(n) for cached data.
