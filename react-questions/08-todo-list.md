# Question 8: Todo List App with complete button

## Problem Statement

Create a Todo List app where you can add items and each item has a Complete button that marks the item complete and disables the button.

## Solution

### Steps:

1. Use state to manage todo list with `text` and `completed` flag.
2. Add new todos via an input.
3. Render todos with Complete buttons.
4. On complete, mark `completed` true and disable button.

### Code Example:

```tsx
import React, { useState } from 'react'

type Todo = {
  id: number
  text: string
  completed: boolean
}

const TodoList: React.FC = () => {
  const [todos, setTodos] = useState<Todo[]>([])
  const [input, setInput] = useState('')

  const addTodo = () => {
    if (!input.trim()) return
    setTodos([...todos, { id: Date.now(), text: input, completed: false }])
    setInput('')
  }

  const completeTodo = (id: number) => {
    setTodos(
      todos.map((todo) =>
        todo.id === id ? { ...todo, completed: true } : todo
      )
    )
  }

  return (
    <div>
      <input
        value={input}
        onChange={(e) => setInput(e.target.value)}
        placeholder='Add task'
      />
      <button onClick={addTodo}>Add</button>

      <ul>
        {todos.map((todo) => (
          <li key={todo.id}>
            {todo.text}
            <button
              onClick={() => completeTodo(todo.id)}
              disabled={todo.completed}>
              Complete
            </button>
          </li>
        ))}
      </ul>
    </div>
  )
}

export default TodoList
```

## Interviewer Tips

- Manage immutability for updates.
- Handle empty input and duplicate tasks.

## Complexity

- Time: O(n) for updating.
- Space: O(n).
