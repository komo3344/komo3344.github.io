---
title: 알고리즘 - 로그파일 재정렬
date: 2021-04-15
category: 알고리즘
tags: 알고리즘
---

[리트코드 937번 문제](https://leetcode.com/problems/reorder-data-in-log-files/)

### 로그를 재정렬하라. 기준은 다음과 같다

1. 로그의 가장 앞 부분은 식별자다.
2. 문자로 구성된 로그가 숫자 로그보다 앞에온다.
3. 식별자는 순서에 영향을 끼치지 않지만, 문자가 동일할 경우 식별자 순으로 한다.
4. 숫자 로그는 입력 순서대로 한다.

| Input                                                                         | Output                                                                        |
| ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| ["dig1 8 1 5 1","let1 art can","dig2 3 6","let2 own kit dig","let3 art zero"] | ["let1 art can","let3 art zero","let2 own kit dig","dig1 8 1 5 1","dig2 3 6"] |
| ["a1 9 2 3 1","g1 act car","zo4 4 7","ab1 off key dog","a8 act zoo"]          | ["g1 act car","a8 act zoo","ab1 off key dog","a1 9 2 3 1","zo4 4 7"]          |

<br><br>

## Solution (람다와 리스트 '+' 연산자 이용)

```python
def reorderLogFiles(logs: List[str]) -> List[str]:
    letters, digits = [], []
    for log in logs:
        if log.split()[0].isdigit():
            digits.append(log)
        else:
            letters.append(log)

    letters.sort(key = lambda x: (x.split()[1:], x.split()[0]))
    return letters + digits
```

python의 sort, sorted에 사용되는 **key** 인자는
**어떤 기준으로 정렬**할지 정할 수 있는데 **리턴값이 있는 함수**가 와야한다.  
여기서는 (로그 내용, 식별자) 2개의 키를 기준으로 정렬한다.
