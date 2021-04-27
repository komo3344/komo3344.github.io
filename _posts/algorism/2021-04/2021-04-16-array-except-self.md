---
title: 알고리즘 - 자신을 제외한 배열의 곱
date: 2021-04-16
category: 알고리즘
tags: 알고리즘
---

[리트코드 238번 문제](https://leetcode.com/problems/product-of-array-except-self/)

### 배열을 입력받아 output[i]가 자신을 제외한 나머지 모든 요소의 곱셈 결과가 되도록 출력하라.

(나눗셈 없이 계산하여아함)

| Input     | Output      |
| --------- | ----------- |
| [1,2,3,4] | [24,12,8,6] |

<br>

### Solution1 ()

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        out = []
        p = 1
        for i in range(0, len(nums)):
            out.append(p)
            p = p * nums[i]

        p = 1
        for i in range(len(nums) - 1, 0 - 1, -1):
            out[i] = p * out[i]
            p = p * nums[i]

        return out
```

요소의 모든 곱을 구한 뒤 자기자신을 나누는 풀이법은 문제 조건에 맞지 않는다.  
따라서 자신을 제외한 왼쪽의 곱셈 결과와 오른쪽의 곱셉 결과를 곱해야한다.  
왼쪽 곱셈 결과 = [1, 1, 2, 6]  
오른쪽 곱셈 결과 = [1, 4, 12, 24]  
왼쪽의 곱셈 결과와 오른쪽 곱셈 결과(거꾸로) 곱하면 된다.
range에서 0 - 1을 하는 이유는 인덱스가 0까지 가기 위해선 -1을 해줘야 하기 때문이다.
