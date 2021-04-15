---
title: 알고리즘 - 로그파일 재정렬
date: 2021-04-14
category: 알고리즘
tags: 알고리즘
---

[리트코드 125번 문제](https://leetcode.com/problems/valid-palindrome/)

### 주어진 문자열이 팰린드롬인지 확인하라. 대소문자를 구분하지 않으며, 영문자와 숫자만을 대상으로 한다.

**팰린드롬** : 뒤집어도 같은 말이 되는 단어 또는 문장

```
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.
```

```
Input: s = "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.
```

## solution1 (리스트로 변환)

```python

def isPalindrome(s: str) -> bool:
    strs = []
    for char in s:
        if char.isalnum():
            strs.append(char.lower())

    while len(strs) > 1:
        if strs.pop(0) != strs.pop():
            return False
    return True
```

## solution2 (데크 자료형을 이용한 최적화)

```python

def isPalindrome(s: str) -> bool:
    strs: Deque = collections.deque()
    for char in s:
        if char.isalnum():
            strs.append(char.lower())

    while len(strs) > 1:
        if strs.popleft() != strs.pop():
            return False
    return True
```

solution1과 비교하면 5배 가까이 더 속도를 높일 수 있다.
이는 리스트의 pop(0)이 O(n)인데 반해, 데크의 popleft()는 O(1)이기 때문이며
각각 n번씩 반복하면 리스트 구현은 O(n^2), 데크 구현은 O(n)으로 성능 차이가 크다.

## solution3 (슬라이싱 사용)

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        s = re.sub('[^a-z0-9]', '', s.lower())
        return s == s[::-1]
```

isalnum()으로 모든 문자를 일일이 점검하지 않고
정규 표현식을 통해 한 번에 처리했다.
[::-1]을 통해 리스트를 뒤집을 수 있다.
코드도 줄어들고 내부적으로 C로 빠르게 구현되어 있어 훨씬 더 좋은 속도를 낸다.
solution2보다 약 2배 정도 더 속도를 높일 수 있다.
