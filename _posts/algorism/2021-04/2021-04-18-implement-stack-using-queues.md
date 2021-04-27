---
title: 알고리즘 - 큐를 이용한 스택구현
date: 2021-04-18
category: 알고리즘
tags: 알고리즘
---

[리트코드 225번 문제](https://leetcode.com/problems/implement-stack-using-queues/)

### 큐를 이용해 다음 연산을 지원하는 스택을 구현하라.

```python
def push(self, x: int) -> None:
        """
        Push element x onto stack.
        """


    def pop(self) -> int:
        """
        Removes the element on top of the stack and returns that element.
        """


    def top(self) -> int:
        """
        Get the top element.
        """


    def empty(self) -> bool:
```

<br><br>

## Solution1

```python
class MyStack:

    def __init__(self):
        self.q = collections.deque()

    def push(self, x: int) -> None:
        self.q.append(x)
        for _ in range(len(self.q) - 1):
            self.q.append(self.q.popleft())

    def pop(self) -> int:
        return self.q.popleft()

    def top(self) -> int:
        return self.q[0]

    def empty(self) -> bool:
        return len(self.q) == 0
```
