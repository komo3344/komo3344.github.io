---
title: 알고리즘 - 스택을 이용한 큐 구현
date: 2021-04-19
category: 알고리즘
tags: 알고리즘
---

### 스택을 이용해 다음 연산을 지원하는 큐를 구현하라.

[리트코드 232번 문제](https://leetcode.com/problems/implement-queue-using-stacks/)

```python
class MyQueue:

    def __init__(self):
        """
        Initialize your data structure here.
        """


    def push(self, x: int) -> None:
        """
        Push element x to the back of queue.
        """


    def pop(self) -> int:
        """
        Removes the element from in front of queue and returns that element.
        """


    def peek(self) -> int:
        """
        Get the front element.
        """


    def empty(self) -> bool:
        """
        Returns whether the
```

## Solution

```python
class MyQueue:

    def __init__(self):
        self.input = []
        self.output = []

    def push(self, x: int) -> None:
        self.input.append(x)

    def pop(self) -> int:
        self.peek()
        return self.output.pop()


    def peek(self) -> int:
        # output이 없으면 모두 재입력
        if not self.output:
            while self.input:
                self.output.append(self.input.pop())
        return self.output[-1]

    def empty(self) -> bool:
        return self.input == [] and self.output == []
```
