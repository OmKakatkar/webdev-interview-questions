# Question 6: Comments Analytics Chart by Post

## Problem Statement

Build a React + TypeScript Application to fetch posts and comments and visualize number of comments per post in a chart.

Requirements:

- Fetch posts and their comments
- Display bar or pie chart showing comment counts per post
- Use `useMemo` to compute counts efficiently
- On desktop, show chart and table side-by-side
- On mobile, stack chart above table

## Solution

### Steps:

1. Fetch posts and comments asynchronously.
2. Use `useMemo` to build comment counts per post.
3. Use a chart library (e.g., Chart.js or Recharts) for visualization.
4. Create responsive layout to switch between desktop and mobile views.

### Code Outline:

```tsx
import React, { useState, useEffect, useMemo } from 'react'
import {
  BarChart,
  Bar,
  XAxis,
  YAxis,
  Tooltip,
  PieChart,
  Pie,
  Cell,
  Legend,
} from 'recharts'

interface Post {
  id: number
  title: string
}

interface Comment {
  postId: number
  id: number
}

const CommentsAnalytics: React.FC = () => {
  const [posts, setPosts] = useState<Post[]>([])
  const [comments, setComments] = useState<Comment[]>([])
  const [isMobile, setIsMobile] = useState(window.innerWidth < 768)

  useEffect(() => {
    Promise.all([
      fetch('https://jsonplaceholder.typicode.com/posts').then((r) => r.json()),
      fetch('https://jsonplaceholder.typicode.com/comments').then((r) =>
        r.json()
      ),
    ]).then(([postsData, commentsData]) => {
      setPosts(postsData)
      setComments(commentsData)
    })

    function handleResize() {
      setIsMobile(window.innerWidth < 768)
    }

    window.addEventListener('resize', handleResize)
    return () => window.removeEventListener('resize', handleResize)
  }, [])

  const commentCounts = useMemo(() => {
    const counts: Record<number, number> = {}
    comments.forEach((c) => {
      counts[c.postId] = (counts[c.postId] || 0) + 1
    })
    return counts
  }, [comments])

  const chartData = posts.map((post) => ({
    name: post.title.slice(0, 15) + '...',
    comments: commentCounts[post.id] || 0,
    postId: post.id,
  }))

  return (
    <div
      style={{ display: 'flex', flexDirection: isMobile ? 'column' : 'row' }}>
      <div style={{ flex: 1 }}>
        <BarChart
          width={isMobile ? 300 : 600}
          height={300}
          data={chartData}>
          <XAxis dataKey='name' />
          <YAxis />
          <Tooltip />
          <Bar
            dataKey='comments'
            fill='#8884d8'
          />
        </BarChart>
      </div>
      <div
        style={{ flex: 1, overflowX: 'auto', paddingLeft: isMobile ? 0 : 20 }}>
        <table style={{ width: '100%', borderCollapse: 'collapse' }}>
          <thead>
            <tr>
              <th>Post Title</th>
              <th># Comments</th>
            </tr>
          </thead>
          <tbody>
            {posts.map((post) => (
              <tr key={post.id}>
                <td>{post.title}</td>
                <td>{commentCounts[post.id] || 0}</td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
    </div>
  )
}

export default CommentsAnalytics
```

## Interviewer Tips

- Use `useMemo` for performance.
- Design responsive layouts with CSS or libraries.
- Chart libraries have their own learning curves.

## Complexity

- Time: O(n) to process comments.
- Space: O(n).
