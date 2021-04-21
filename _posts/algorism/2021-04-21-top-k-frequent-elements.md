---
title: 알고리즘 - 상위 K 빈도 요소
date: 2021-04-21
category: 알고리즘
tags: 알고리즘
---

[리트코드 347번 문제](https://leetcode.com/problems/top-k-frequent-elements/)

### 상위 k번 이상 등장하는 요소를 추출하라

| Input                       | Output |
| --------------------------- | ------ |
| nums = [1,1,1,2,2,3], k = 2 | [1,2]  |

## Solution1 (Counter를 이용한 풀이) - 처음 내가 한 것

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        counter = collections.Counter(nums)
        return [i[0] for i in counter.most_common(k)]
```

## Solution2 (Counter를 음수 순 추출)

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        freqs = collections.Counter(nums)
        freqs_heap = []
        for f in freqs:
            heapq.heappush(freqs_heap, (-freqs[f], f))

        topk = list()
        for _ in range(k):
            topk.append(heapq.heappop(freqs_heap)[1])

        return topk
```

요소의 값을 키로 하는 해시 테이블을 만들고 여기에 빈도 수를 저장한다.  
우선순위 큐를 이용해 상위 k만큼 추출하면 k번 이상 등장하는 요소를 추출할 수 있다.

<br><br>

## Solution3 (파이썬다운 방식)

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        return list(zip(*collections.Counter(nums).most_common(k)))[0]
```

Counter에 담긴 요소를 언팩(unpack)하여 zip으로 묶었다.  
[(1, 3), (2, 2)] -> [(1, 2), (3, 2)]
