---
title: 알고리즘 - 유효한 괄호
date: 2021-04-18
category: 알고리즘
tags: 알고리즘
---

[리트코드 20번 문제](https://leetcode.com/problems/valid-parentheses/)

### 괄호로 된 입력값이 올바른지 판별하라.

| Input        | Output |
| ------------ | ------ |
| s = "()[]{}" | true   |
| s = "{[]}"   | true   |
| s = "([)]"   | false  |

<br><br>

## Solution1 (스택 일치 여부 판별)

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        table = {
            ')': '(',
            ']': '[',
            '}': '{'
        }

        for char in s:
            if char not in table:
                stack.append(char)
            elif not stack or table[char] != stack.pop():
                return False
        return len(stack) == 0
```
