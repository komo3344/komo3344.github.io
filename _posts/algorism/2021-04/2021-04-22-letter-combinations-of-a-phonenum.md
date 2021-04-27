---
title: 알고리즘 - 전화번호 문자 조합
date: 2021-04-22
category: 알고리즘
tags: 알고리즘
---

[리트코드 17번 문제](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

### 1~9까지 숫자가 주어졌을 때 전화번호로 조합 가능한 모든 문자를 출력하라. (그림은 문제 링크 참조)

Input: digits = "23"  
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
<br><br>

## Solution (모든 조합 DFS 탐색)

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        def dfs(index, path):
            # 끝까지 탐색하면 백트래킹
            if len(path) == len(digits):
                result.append(path)
                return

            # 입력값 자릿수 단위 반복
            for i in range(index, len(digits)):
            # 숫자에 해당하는 모든 문자열 반복
                for j in dic[digits[i]]:
                    dfs(i + 1, path + j)

        # 예외처리
        if not digits:
            return []

        dic = {
            "2": "abc", "3": "def", "4": "ghi", "5": "jkl",
            "6": "mno", "7": "pqrs", "8": "tuv", "9": "wxyz"
        }
        result = []
        dfs(0, "")

        return result
```
