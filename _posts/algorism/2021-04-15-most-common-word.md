---
title: 알고리즘 - 가장 흔한 단어
date: 2021-04-15
category: 알고리즘
tags: 알고리즘
---

[리트코드 819번 문제](https://leetcode.com/problems/most-common-word/)

### 금지된 단어를 제외한 가장 흔하게 등장하는 단어를 출력하라. 대소무낮 구분을 하지 않으며, 구두점(마침표, 쉼표 등) 또한 무시한다.

| Input                                                              | Output |
| ------------------------------------------------------------------ | ------ |
| "Bob hit a ball, the hit BALL flew far after it was hit.", ["hit"] | "ball" |
| "a.", []                                                           | "a"    |

## Soultion1 (리스트 컴프리헨션)

```python
def mostCommonWord(paragraph: str, banned: List[str]) -> str:
    words = [word for word in re.sub('r[^\w]', ' ', paragraph).lower().split() if word not in banned]

    counts = collections.defaultdict(int)   # int 기본값이 자동으로 부여됨
    for word in words:
        counts[word] += 1

    return max(counts, key=counts.get)
```

## Soultion2 (리스트 컴프리헨션, Counter 객체 사용)

```python
def mostCommonWord(paragraph: str, banned: List[str]) -> str:
    words = [word for word in re.sub('r[^\w]', ' ', paragraph).lower().split() if word not in banned]

    counts = collections.Counter(words)
    return counts.most_common(1)[0][0]

```
