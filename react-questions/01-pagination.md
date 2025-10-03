# Question 1: API Pagination with Next, Previous, and Page Size Dropdown

## Problem Statement

Create a React component that invokes an API to fetch data and displays it in paginated form. Include Next and Previous buttons to navigate pages, along with a dropdown to select how many records to show per page.

## Solution

### Steps:

1. Fetch data on component mount using `useEffect`.
2. Maintain state variables for `currentPage` and `pageSize`.
3. Calculate total pages and slice data for current page.
4. Next and Previous buttons update page state within valid bounds.
5. Dropdown menu updates page size and resets page to 1.

### Code Example:

```tsx
import React, { useEffect, useState } from 'react'

type DataType = {
  id: number
  name: string
}

const PaginatedData: React.FC = () => {
  const [data, setData] = useState<DataType[]>([])
  const [currentPage, setCurrentPage] = useState(1)
  const [pageSize, setPageSize] = useState(10)

  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/users')
      .then((res) => res.json())
      .then(setData)
      .catch(console.error)
  }, [])

  const totalPages = Math.ceil(data.length / pageSize)

  const handleNext = () => {
    if (currentPage < totalPages) setCurrentPage(currentPage + 1)
  }

  const handlePrev = () => {
    if (currentPage > 1) setCurrentPage(currentPage - 1)
  }

  const handlePageSizeChange = (e: React.ChangeEvent<HTMLSelectElement>) => {
    setPageSize(Number(e.target.value))
    setCurrentPage(1)
  }

  const startIndex = (currentPage - 1) * pageSize
  const paginatedData = data.slice(startIndex, startIndex + pageSize)

  return (
    <div>
      <select
        value={pageSize}
        onChange={handlePageSizeChange}>
        {[5, 10, 20].map((size) => (
          <option
            key={size}
            value={size}>
            {size} per page
          </option>
        ))}
      </select>

      <ul>
        {paginatedData.map((item) => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>

      <button
        onClick={handlePrev}
        disabled={currentPage === 1}>
        Previous
      </button>
      <button
        onClick={handleNext}
        disabled={currentPage === totalPages}>
        Next
      </button>

      <p>
        Page {currentPage} of {totalPages}
      </p>
    </div>
  )
}

export default PaginatedData
```

## Interviewer Tips & Traps

- Handle empty or very small data gracefully.
- Discuss client-side vs. server-side pagination tradeoffs.
- Reset current page when page size changes.
- Consider accessibility for buttons and dropdowns.

## Complexity

- Time: O(n) to slice data per page.
- Space: O(n) to store all fetched data.
