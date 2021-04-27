---
title: 알고리즘 - 섬의 개수
date: 2021-04-22
category: 알고리즘
tags: 알고리즘
---

[리트코드 20번 문제](https://leetcode.com/problems/number-of-islands/)

### 1을 육지로, 0을 물로 가정한 그리드 맵이 주어졌을때, 섬의 개수를 계산하라.(연결되어 있는 1의 덩어리 개수를 구하라)

Input

[  
["1","1","0","0","0"],  
["1","1","0","0","0"],  
["0","0","1","0","0"],  
["0","0","0","1","1"]
]

Output: 3

## Solution1 (DFS)

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        def dfs(i, j):
            # 더 이상 땅이 아닌 경우
            if i < 0 or i >= len(grid) or j < 0 or j >= len(grid[0]) or grid[i][j] != '1':
                return
            # 탐색 한 것은 다른 값으로 바꿈
            grid[i][j] = '#'
            print(i, j)
            dfs(i + 1, j)
            dfs(i - 1, j)
            dfs(i, j + 1)
            dfs(i, j - 1)

        count = 0
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == '1':
                    dfs(i, j)
                    count += 1
        return count
```
