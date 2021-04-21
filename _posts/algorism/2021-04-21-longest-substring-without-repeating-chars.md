---
title: 알고리즘 - 중복 문자 없는 가장 긴 부분 문자열
date: 2021-04-21
category: 알고리즘
tags: 알고리즘
---

[리트코드 3번 문제](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

### 중복 문자가 없는 가장 긴 부분 문자열의 길이를 리턴하라.

| Input          | Output |
| -------------- | ------ |
| s = "abcabcbb" | 3      |
| s = "bbbbb"    | 1      |
| s = "pwwkew"   | 3      |

<br><br>

## Solution(슬라이딩 윈도우와 투포인터로 사이즈 조절)

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        used = {}
        max_length = start = 0
        for index, char in enumerate(s):
            if char in used and start <= used[char]:
                start = used[char] + 1
            else:
                max_length = max(max_length, index - start + 1)
            used[char] = index

        return max_length
```

start는 첫번째 포인터 역할,  
used는 현재 문자를 키로 하고 인덱스를 값으로 하는 끝 포인터 역할을 한다.

현재 문자의 인덱스 값을 저장할텐데  
현재 문자가 중복되서 사용됬다면 첫번째 포인터를 앞으로 이동시킨다.  
아니라면 현재 저장된 최대값과 현재 인덱스 - 시작 인덱스 + 1 한 값을 비교하여 큰 값을 max_length에 넣는다.

중복값처리와 포인터 역할을 하는 해시 테이블을 만든다는 생각을 하는 것이 키포인트였던것 같다.
