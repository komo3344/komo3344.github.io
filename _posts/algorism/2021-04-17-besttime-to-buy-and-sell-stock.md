---
title: 알고리즘 - 주식을 사고팔기 가장 좋은 시점
date: 2021-04-17
category: 알고리즘
tags: 알고리즘
---

[리트코드 121번 문제](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

### 한 번의 거래로 낼 수 있는 최대 이익을 산출하라.

| Input         | Output |
| ------------- | ------ |
| [7,1,5,3,6,4] | 5      |

<br><br>

## Solution1 (브루트 포스로 계산) -> 시간초과

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        max_price = 0
        for i, price in enumerate(prices):
            for j in range(i, len(prices)):
                max_price = max(prices[j] - price, max_price)
        return max_price
```

## Solution2 (저점과 현재 값과의 차이 계산)

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        min_price = sys.maxsize
        profit = 0
        for price in prices:
            min_price = min(min_price, price)
            profit = max(profit, price - min_price)
        return profit
```

데이터를 그래프로 그리면 직관적인 풀이가 떠오를 수 있다.  
시스템의 최소, 최댓값 지정방법에 sys.maxsize를 활용할 수 있다.  
파이썬의 숫자형은 **임의 정밀도**를 지원한다고 한다. 사실상 무한대의 값을 지정 할 수 있기 때문에 얼마든지 더 큰 값이 들어와 최솟값이 교체되지 않을 수 있기 때문이다.  
사실상 sys.maxsize로 선언하는 것도 파이썬에서는 큰 의미가 없지만 코딩테스트는 모든 언어에 대응하는 공통된 테스트 케이스로 구성되어 있기 때문에 이정도 처리하는 정도도 충분하다.  
또는 문제에 나와있는 최대, 최솟값을 처리해주면 된다.
