# Question 11: Component with 3 tabs and pagination within a tab

## Problem Statement

Create a component with 3 tabs. Each tab has large data with pagination to navigate within that tab.

## Solution

### Steps:

1. Maintain active tab state.
2. Maintain pagination state per tab.
3. Show data sliced based on current page and page size per tab.
4. Switch tabs while preserving states.

### Code Example:

```tsx
import React, { useState } from 'react';

type TabData = {
  id: number;
  content: string;
};

const dataPerTab: Record<string, TabData[]> = {
  tab1: Array.from({ length: 50 }, (_, i) => ({ id: i, content: \`Tab 1 - Item ${i + 1}\` })),
  tab2: Array.from({ length: 30 }, (_, i) => ({ id: i, content: \`Tab 2 - Item ${i + 1}\` })),
  tab3: Array.from({ length: 40 }, (_, i) => ({ id: i, content: \`Tab 3 - Item ${i + 1}\` })),
};

const PAGE_SIZE = 10;

const TabsWithPagination: React.FC = () => {
  const [activeTab, setActiveTab] = useState('tab1');
  const [pagePerTab, setPagePerTab] = useState<Record<string, number>>({ tab1: 1, tab2: 1, tab3: 1 });

  const handlePageChange = (tab: string, page: number) => {
    setPagePerTab(prev => ({ ...prev, [tab]: page }));
  };

  const currentData = dataPerTab[activeTab];
  const currentPage = pagePerTab[activeTab];

  const paginatedData = currentData.slice((currentPage - 1) * PAGE_SIZE, currentPage * PAGE_SIZE);
  const totalPages = Math.ceil(currentData.length / PAGE_SIZE);

  return (
    <div>
      <div>
        {['tab1', 'tab2', 'tab3'].map(tab => (
          <button key={tab} onClick={() => setActiveTab(tab)} disabled={activeTab === tab}>
            {tab.toUpperCase()}
          </button>
        ))}
      </div>

      <ul>
        {paginatedData.map(item => (
          <li key={item.id}>{item.content}</li>
        ))}
      </ul>

      <button onClick={() => handlePageChange(activeTab, Math.max(currentPage - 1, 1))} disabled={currentPage === 1}>
        Previous
      </button>
      <button onClick={() => handlePageChange(activeTab, Math.min(currentPage + 1, totalPages))} disabled={currentPage === totalPages}>
        Next
      </button>

      <p>Page {currentPage} of {totalPages}</p>
    </div>
  );
};

export default TabsWithPagination;
```

## Interviewer Tips

- Preserve states for each tab independently.
- Avoid re-fetching if using API.

## Complexity

- Time: O(n) for pagination slice.
- Space: O(n) for data.
