---
title: 알고리즘 - 배열 파티션 1
date: 2021-04-16
category: 알고리즘
tags: 알고리즘
---

### n개의 페어를 이용한 min(a, b)의 합으로 만들 수 있는 가장 큰 수를 출력하라.

[리트코드 561번 문제](https://leetcode.com/problems/array-partition-i/)

| Input     | Output |
| --------- | ------ |
| [1,4,3,2] | 4      |

ex) min(1,2) + min(3,4) = 4

<br>

### Solution1 (오름차순)

```python
class Solution:
    def arrayPairSum(self, nums: List[int]) -> int:
        sum = 0
        pair = []
        nums.sort()

        for n in nums:
            pair.append(n)
            if len(pair) == 2:
                sum += min(pair)
                pair = []

        return sum
```

### Solution2 (짝수 번째 값 계산)

```python
class Solution:
    def arrayPairSum(self, nums: List[int]) -> int:
        sum = 0
        nums.sort()

        for i, n in enumerate(nums):
            if i % 2 == 0:
                sum += n
        return sum
```

## Solution3 (파이썬다운 방식)

```python
class Solution:
    def arrayPairSum(self, nums: List[int]) -> int:
        return sum(sorted(nums)[::2])
```

슬라이싱을 사용한 덕분에 성능이 가장 좋다.
