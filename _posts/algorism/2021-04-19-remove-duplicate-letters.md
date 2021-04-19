---
title: 알고리즘 - 중복 문자 제거
date: 2021-04-18
category: 알고리즘
tags: 알고리즘
---

[리트코드 316번 문제](https://leetcode.com/problems/remove-duplicate-letters/)

### 중복된 문자를 제외하고 사전식 순서로 나열하라.

| Input          | Output |
| -------------- | ------ |
| s = "bcabc"    | "abc"  |
| s = "cbacdcbc" | "acdb" |

<br><br>

## Solution1 (스택을 이용한 문자 제거)

```python
class Solution:
    def removeDuplicateLetters(self, s: str) -> str:
        counter, stack = collections.Counter(s), []

        for char in s:
            counter[char] -= 1
            if char in stack:
                continue
            while stack and char < stack[-1] and and counter[stack[-1]] > 0:
                stack.pop()
            stack.append(char)

        return ''.join(stack)
```

현재 문자가 스택에 쌓여있는 문자(이전 문자보다 앞선 문자)이고, 뒤에 다시 붙일 문자가 남아 있다면(카운터가 0 이상 이라면) 쌓아둔 걸 꺼내서 없앤다.
<br><br>

## Solution2 (재귀를 이용한 풀이) \*\* TODO

```python
class Solution:
    def removeDuplicateLetters(self, s: str) -> str:
        # 집합으로 정렬
        for char in sorted(set(s)):
            suffix = s[s.index(char):]
            # 전체 집합과 접미사 집합이 일치할 때 분리 진행
            if set(s) == set(suffix):
                return char + self.removeDuplicateLetters(suffix.replace(char, ''))
        return ''

```
