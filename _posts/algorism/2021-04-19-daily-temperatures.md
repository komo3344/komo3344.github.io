---
title: 알고리즘 - 일일 온도
date: 2021-04-18
category: 알고리즘
tags: 알고리즘
---

[리트코드 739번 문제](https://leetcode.com/problems/daily-temperatures/)

### 매일의 화씨 온도 리스트 T를 입력받아서, 더 따뜻한 날씨를 위해서는 며칠을 더 기다려야 하는지를 출력하라.

| Input                                | Output                   |
| ------------------------------------ | ------------------------ |
| T = [73, 74, 75, 71, 69, 72, 76, 73] | [1, 1, 4, 2, 1, 1, 0, 0] |

<br><br>

## Solution1

```python
class Solution:
    def dailyTemperatures(self, T: List[int]) -> List[int]:
        answer, stack = [0] * len(T), []

        for i, cur in enumerate(T):
            while stack and cur > T[stack[-1]]:
                last = stack.pop()
                answer[last] = i - last
            stack.append(i)
        return answer
```

현재의 인덱스를 계속 스택에 쌓아두다가, 이전보다 상승하는 지점에서 현재의 온도와 스택에 쌓아둔 인덱스 지점의 온도 차이를 비교해서  
더 높다면 스택의 값을 팝으로 꺼내고 현재 인덱스와 스택에 쌓아둔 인덱스의 차이를 정답으로 처리한다.
