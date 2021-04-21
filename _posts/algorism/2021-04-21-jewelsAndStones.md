---
title: 알고리즘 - 보석과 돌
date: 2021-04-21
category: 알고리즘
tags: 알고리즘
---

[리트코드 771번 문제](https://leetcode.com/problems/jewels-and-stones/)

### 돌에는 보석이 몇 개나 있을까? 대소문자는 구분한다.

| Input                             | Output |
| --------------------------------- | ------ |
| jewels = "aA", stones = "aAAbbbb" | 3      |

<br><br>

## Solution1 (해시 테이블을 이용한 풀이)

```python
class Solution:
    def numJewelsInStones(self, jewels: str, stones: str) -> int:
        freqs = {}
        count = 0

        for char in stones:
            if char not in freqs:
                freqs[char] = 1
            else:
                freqs[char] += 1

        for char in jewels:
            if char in freqs:
                count += freqs[char]

        return count
```

돌 각각의 개수를 모두 헤아린 다음 보석의 각 요소를 키로 하는 각 개수를 합산하면 된다.  
해시 테이블로 풀이할 수 있는 전형적인 문제이다.

<br><br>

## Solution2 (defaultdict를 이용한 비교 생략)

```python
class Solution:
    def numJewelsInStones(self, jewels: str, stones: str) -> int:
        freqs = collections.defaultdict(int)
        count = 0

        for char in stones:
            freqs[char] += 1

        for char in jewels:
            if char in freqs:
                count += freqs[char]

        return count
```

존재하지 않는 키에 대해 디폴트를 리턴해 주는 defaultdict을 사용하여 비교하는 과정을 생략했다.

<br><br>

## Solution3 (Counter로 계산 생략)

```python
class Solution:
    def numJewelsInStones(self, jewels: str, stones: str) -> int:
        freqs = collections.Counter(stones)
        count = 0

        for char in jewels:
            count += freqs[char]

        return count
```

Counter는 존재하지 않는 키의 경우 KeyError를 발생시키는 게 아니라 0을 출력해 주기 때문에 defaultdict과 마찬가지로 예외 처리를 할 필요가 없다.

<br><br>

## Solution4 (파이썬다운 방식)

```python
class Solution:
    def numJewelsInStones(self, jewels: str, stones: str) -> int:
        return sum(s in jewels for s in stones)
# [s for s in stones] => ['a', 'A', 'A', 'b', 'b', 'b', 'b']
# [s in jewels for s in stones] => [True, True, True, False, False, False, False]
# sum(s in jewels for s in stones) => True의 개수를 세어준다
# 리스트 컴프리헨션을 의미하는 앞뒤의 대괄호는 제거할 수 있다.
```

<br><br>
