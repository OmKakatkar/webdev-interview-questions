# Question 12: Implement Error Boundaries in Class Components

## Problem Statement

Create a class component error boundary that catches JavaScript errors in children and displays fallback UI.

## Solution

### Steps:

1. Use `componentDidCatch` and `getDerivedStateFromError` lifecycle methods.
2. Render fallback UI on error.
3. Wrap any child component with error boundary.

### Code Example:

```tsx
import React, { Component, ReactNode } from 'react'

type Props = { children: ReactNode }
type State = { hasError: boolean }

class ErrorBoundary extends Component<Props, State> {
  constructor(props: Props) {
    super(props)
    this.state = { hasError: false }
  }

  static getDerivedStateFromError(_: Error) {
    return { hasError: true }
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    console.error('ErrorBoundary caught:', error, errorInfo)
  }

  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>
    }
    return this.props.children
  }
}

export default ErrorBoundary
```

## Interviewer Tips

- Explain error boundary lifecycle.
- Discuss what error boundaries cannot catch.

## Complexity

- N/A
