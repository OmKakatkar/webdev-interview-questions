# Question 7: Recursive functions to search nested tree nodes by id or className

## Problem Statement

Given a nested tree where each node has optional `id`, array of `classList`, and nested `children`, write recursive functions to search for nodes by `id` and by `className`.

## Solution

### Steps:

1. Traverse the tree recursively.
2. For search by id, return the node matching id.
3. For search by className, collect all nodes having the className.

### Code Example:

```tsx
type TreeNode = {
  id?: string
  classList: string[]
  children: TreeNode[]
}

function findNodeById(node: TreeNode, id: string): TreeNode | null {
  if (node.id === id) return node
  for (const child of node.children) {
    const result = findNodeById(child, id)
    if (result) return result
  }
  return null
}

function findNodesByClassName(node: TreeNode, className: string): TreeNode[] {
  let matches: TreeNode[] = []
  if (node.classList.includes(className)) {
    matches.push(node)
  }
  for (const child of node.children) {
    matches = matches.concat(findNodesByClassName(child, className))
  }
  return matches
}
```

## Interviewer Tips

- Handle undefined or empty children.
- Recursive depth considerations.

## Complexity

- Time: O(n) where n is total nodes.
- Space: O(n) for call stack and results.
