---
title: 알고리즘 - 빗물 트래핑
date: 2021-04-16
category: 알고리즘
tags: 알고리즘
---

### 높이를 입력받아 비 온 후 얼마나 많은 물이 쌓일 수 있는지 계산하라

[리트코드 42번 문제](https://leetcode.com/problems/trapping-rain-water/)

| Input                     | Output |
| ------------------------- | ------ |
| [0,1,0,2,1,0,1,3,2,1,2,1] | 6      |

<br>

### Solution1 (투 포인터를 최대로 이동)

```python
def trap(height: List[int]) -> int:
    if not height:
        return 0

    volume = 0
    left, right = 0, len(height) - 1
    left_max, right_max = height[left], height[right]

    while left < right:
        left_max, right_max = max(left_max, height[left]), max(right_max, height[right])
        if left_max <= right_max:
            volume += left_max - height[left]
            left += 1
        else:
            volume += right_max - height[right]
            right -= 1

        return volume
```

양쪽의 포인터를 이동시키면서 좌우 기둥 최대 높이에서 현재 높이와의 차이만큼 물 높이 volume을 더해 나간다.

<br>

각 포인터 인덱스, 최대 높이, 물의 양 변수의 흐름

| left | right | left_max | right_max | volume | 분기 |
| ---- | ----- | -------- | --------- | ------ | ---- |
| 0    | 11    | 0        | 1         | 0      | if   |
| 1    | 11    | 1        | 1         | 0      | if   |
| 2    | 11    | 1        | 1         | 1      | if   |
| 3    | 11    | 2        | 1         | 1      | else |
| 3    | 10    | 2        | 1         | 1      | if   |
| 4    | 10    | 2        | 2         | 2      | if   |
| 5    | 10    | 2        | 2         | 4      | if   |
| 6    | 10    | 2        | 2         | 5      | else |
| 7    | 10    | 3        | 2         | 5      | else |
| 7    | 9     | 3        | 2         | 6      | else |
| 7    | 8     | 3        | 2         | 6      |
| 7    | 7     |

### Solution2 (스택 쌓기)

```python
def trap(height: List[int]) -> int:
    volume = 0
    stack = []

    for i in range(len(height)):
        while stack and height[i] > height[stack[-1]]:
            top = stack.pop()
            if not len(stack):
                break

            distance = i - stack[-1] - 1
            waters = min(height[i], height[stack[-1]]) - height[top]

            volume += distance * waters

        stack.append(i)

    return volume
```

스택이 비어있지 않고 현재 높이가 이전의 높이보다 큰 경우에 계산함  
거리는 현재 인덱스 - 스택에 있는 인덱스 - 1을 해준다.  
문제의 3번 인덱스 기준으로 스택에는 1, 2 인덱스가 있지만 2번 인덱스는 pop으로 빠져나갔기 때문에 3 - 1 을 해주고 1~3의 거리를 구해야 하기 때문에 -1을 더 빼준다.

높이는 현재 높이와 스택 마지막의 높이의 최솟값을 구한 뒤 top의 높이 만큼 빼준다.  
따라서 물의 양은 volume += distance \* waters로 구할 수 있다.  
상당히 어려운 문제이고 투 포인터로 푸는 방식이 이해하기 더 쉬웠다.
