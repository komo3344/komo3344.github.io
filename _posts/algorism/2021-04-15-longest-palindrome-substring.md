---
title: 알고리즘 - 가장 긴 팰린드롬 부분 문자열
date: 2021-04-15
category: 알고리즘
tags: 알고리즘
---

[리트코드 5번 문제](https://leetcode.com/problems/longest-palindromic-substring/)

### 가장 긴 팰린드롬 부분 문자열을 출력하라.

| Input   | Output |
| ------- | ------ |
| "babad" | "bab"  |
| "cbbd"  | "bb"   |
| "a"     | "a"    |
| "ac"    | "a"    |

<br><br>

## Solution (중앙을 중심으로 확장)

```python
def longestPalindrome(s: str) -> str:
    # 1) 팰린드롬 판별 및 포인트 확장
    def expand(left:int, right:int) -> str:
        while left >= 0 and right < len(s) and s[left] == s[right]:
            left -= 1
            right += 1
        return s[left + 1:right]

    # 2) 해당사항이 없을 경우 리턴
    if len(s) < 2 or s == s[::-1]:
        return s

    result = ''
    # 3) 슬라이싱 윈도우 우측으로 이동
    for i in range(len(s) - 1):
        result = max(result, expand(i, i+1), expand(i, i+2), key=len)
    return result
```
