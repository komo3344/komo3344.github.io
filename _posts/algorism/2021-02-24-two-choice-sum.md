---
title: 알고리즘 - 두 개 뽑아서 더하기
date: 2021-02-24
category: 알고리즘
tags: 알고리즘
permalink: /algorism/:year/:month/:day/:title/
---

## 두 개 뽑아서 더하기

정수 배열 numbers가 주어집니다. numbers에서 서로 다른 인덱스에 있는 두 개의 수를 뽑아 더해서 만들 수 있는 모든 수를 배열에 오름차순으로 담아 return 하도록 solution 함수를 완성해주세요.
<br>

## 입출력 예

| numbers     | result        |
| :---------- | :------------ |
| [2,1,3,4,1] | [2,3,4,5,6,7] |
| [5,0,2,7]   | [2,5,7,9,12]  |

<br><br>

## 나의 코드

```python
def solution(numbers):
    answer = []
    for i in range(len(numbers)):
        for j in range(i+1, len(numbers)):
            answer.append(numbers[i]+numbers[j])
    return sorted(list(set(answer)))
```

## 다른 사람의 코드

```python
from itertools import combinations

def solution(numbers):
    answer = []
    l = list(combinations(numbers, 2))
    print(l)

    for i in l:
        print(i)
        answer.append(i[0]+i[1])
    answer = sorted(list(set(answer)))


    return answer
```

## 알게된 점

1. set은 중복제거와 정렬까지 해준다.
   중복은 제거하되 순서를 유지하고 싶다면 아래와 같이 사용 할 수 있다.

```python
import collections

x = [7, 5, 3, 3, 4, 1, 2, 2]

x = collections.OrderedDict.fromkeys(x).keys()

# x = [7, 5, 3, 4, 1, 2]
```

2. python의 **combinations**을 알게되었다.
   **conbinations**는 리스트 조합의 결과를 구할 때 사용된다.

3. 정렬을 위해 사용되는 함수는 sort, sorted가 있는데 sort는 리스트 자체를 정렬(원본 수정)하지만 sorted는 정렬된 **새로운 객체**를 생성한다(원본 유지)
