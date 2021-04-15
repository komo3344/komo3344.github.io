---
title: 알고리즘 - 문자열 뒤집기
date: 2021-04-14
category: 알고리즘
tags: 알고리즘
---

[리트코드 344번 문제](https://leetcode.com/problems/reverse-string/)

### 문자열을 뒤집는 함수를 작성하라. 입력값은 문자 배열이며, 리턴 없이 리스트 내부를 직접 조작하라.

| Input                     | Output                    |
| ------------------------- | ------------------------- |
| ["h","e","l","l","o"]     | ["o","l","l","e","h"]     |
| ["H","a","n","n","a","h"] | ["h","a","n","n","a","H"] |

<br>

## Solution1 (투 포인터를 이용한 스왑)

```python
def reverseString(s: List[str]) -> None:
    left, right = 0, len(s) - 1
    while left < right:
        s[left], s[right] = s[right], s[left]
        left += 1
        right -= 1
```

## Solution2 (파이썬다운 방식)

```python
def reverseString(s: List[str]) -> None:
    s.reverse()
```
