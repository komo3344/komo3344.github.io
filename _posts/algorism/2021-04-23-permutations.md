---
title: 알고리즘 - 순열
date: 2021-04-23
category: 알고리즘
tags: 알고리즘
---

[리트코드 17번 문제](https://leetcode.com/problems/permutations/)

### 서로 다른 정수를 입력받아 가능한 모든 순열을 리턴하라.

| Input          | Output                                            |
| -------------- | ------------------------------------------------- |
| nums = [1,2,3] | [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]] |

<br><br>

## Solution (DFS를 활용한 순열 생성)

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        results = []
        prev_elements = []

        def dfs(elements):
            # 리프 노드일 때 결과 추가
            if len(elements) == 0:
                # prev_elements를 하게되면 결과 값이 추가되는 것이 아니라
                # prev_elements에 대한 참조가 추가되고 참조된 값이 변경될 경우 같이 바뀐다.
                # 따라서 값을 복사하는 형태로 참조 관계를 잦지 않도록 처리해야한다.
                results.append(prev_elements[:])

            # 순열 생성 재귀 호출
            for e in elements:
                next_elements = elements[:]
                next_elements.remove(e)

                prev_elements.append(e)
                dfs(next_elements)
                prev_elements.pop()

        dfs(nums)
        return results
```

<br><br>

## Solution (itertools 모듈 사용)

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        return list(itertools.permutations(nums))
```
