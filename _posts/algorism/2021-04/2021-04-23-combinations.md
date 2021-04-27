---
title: 알고리즘 - 조합
date: 2021-04-23
category: 알고리즘
tags: 알고리즘
---

[리트코드 77번 문제](https://leetcode.com/problems/combinations/)

### 전체 수 n을 입력받아 k개의 조합을 리턴하라.

| Input        | Output                                     |
| ------------ | ------------------------------------------ |
| n = 4, k = 2 | [[2,4], [3,4], [2,3], [1,2], [1,3] [1,4],] |

<br><br>

## Solution1 (DFS로 K개 조합 생성)

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        results = []

        def dfs(elements, start: int, k: int):
            if k == 0:
                results.append(elements[:])
                return

            # 자신 이전의 모든 값을 고정하여 재귀호출
            for i in range(start, n + 1):
                elements.append(i)
                dfs(elements, i + 1, k - 1)
                elements.pop()

        dfs([], 1, k)
        return results
```

## Solution2 (itertools 모듈 사용)

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        return list(itertools.combinations(range(1, n + 1), k))
```
