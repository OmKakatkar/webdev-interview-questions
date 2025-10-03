# Question 5: Display first 10 album titles with checkboxes and enable submit after at least 5 selected. Capitalize second letter of every word.

## Problem Statement

Invoke an API to fetch albums, display first 10 titles with checkboxes, enable submit button when at least 5 items are selected, and capitalize the second letter of every word in the title.

## Solution

### Steps:

1. Fetch albums from API.
2. Display first 10 album titles with checkboxes.
3. Track selected items in state.
4. Enable submit button if at least 5 selected.
5. Function to capitalize the second letter of every word.

### Code Example:

```tsx
import React, { useEffect, useState } from 'react'

type Album = { id: number; title: string }

const capitalizeSecondLetter = (str: string): string => {
  return str
    .split(' ')
    .map((word) => {
      if (word.length < 2) return word
      return word[0] + word[1].toUpperCase() + word.slice(2)
    })
    .join(' ')
}

const AlbumSelector: React.FC = () => {
  const [albums, setAlbums] = useState<Album[]>([])
  const [selected, setSelected] = useState<Set<number>>(new Set())

  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/albums')
      .then((res) => res.json())
      .then((data: Album[]) => setAlbums(data.slice(0, 10)))
      .catch(console.error)
  }, [])

  const toggleSelection = (id: number) => {
    setSelected((prev) => {
      const newSet = new Set(prev)
      if (newSet.has(id)) newSet.delete(id)
      else newSet.add(id)
      return newSet
    })
  }

  return (
    <div>
      <ul>
        {albums.map((album) => (
          <li key={album.id}>
            <label>
              <input
                type='checkbox'
                checked={selected.has(album.id)}
                onChange={() => toggleSelection(album.id)}
              />
              {capitalizeSecondLetter(album.title)}
            </label>
          </li>
        ))}
      </ul>
      <button disabled={selected.size < 5}>Submit</button>
    </div>
  )
}

export default AlbumSelector
```

## Interviewer Tips

- Use Set for efficient selection lookup.
- Capitalize logic as per instructions exactly.

## Complexity

- Time: O(n) per render.
- Space: O(n) for state.
