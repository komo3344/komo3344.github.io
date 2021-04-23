---
title: 알고리즘 - 조합의 합
date: 2021-04-23
category: 알고리즘
tags: 알고리즘
---

### 숫자 집합 candidates를 조합하여 합이 target이 되는 원소를 나열하라. 각 원소는 중복으로 나열 가능하다.

[리트코드 39번 문제](https://leetcode.com/problems/combination-sum/)

| Input                              | Output                    |
| ---------------------------------- | ------------------------- |
| candidates = [2,3,6,7], target = 7 | [[2,2,3],[7]]             |
| candidates = [2,3,5], target = 8   | [[2,2,2,2],[2,3,3],[3,5]] |

<br><br>

## Solution

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        result = []
        # csum: 합을 갱신, index: 자기자신을 포함하는 순서, path: 지금까지의 탐색 경로
        def dfs(csum, index, path):
            # 목표값을 초과한 경우로 탐색을 종료
            if csum < 0 :
                return
            # csum의 초기값은 target이기 때문에 csum의 0은 target과 일치하는 정답
            if csum == 0:
                result.append(path)
                return

            # 자신 부터 하위 원소 까지의 나열 재귀 호출
            for i in range(index, len(candidates)):
                dfs(csum - candidates[i], i, path + [candidates[i]])

        dfs(target, 0 , [])
        return result
```
