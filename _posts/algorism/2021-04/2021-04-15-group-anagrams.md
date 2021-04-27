---
title: 알고리즘 - 그룹 애너그램
date: 2021-04-15
category: 알고리즘
tags: 알고리즘
---

[리트코드 49번 문제](https://leetcode.com/problems/group-anagrams/)

## 문자열 배열을 받아 애너그램 단위로 그풉핑하라.

\*애너그램: 문자를 재배열하여 다른 뜻을 가진 언어로 바꾸는 것  
<br>

| Input                                 | Output                                      |
| ------------------------------------- | ------------------------------------------- |
| ["eat","tea","tan","ate","nat","bat"] | [["bat"],["nat","tan"],["ate","eat","tea"]] |
| [""]                                  | [[""]]                                      |
| ["a"]                                 | [["a"]]                                     |

<br>

### Solution(정렬하여 딕셔너리에 추가)

```python
def groupAnagrams(strs: List[str]) -> List[List[str]]:
    anagrams = collections.defaultdict(list)
    for word in strs:
        anagrams[''.join(sorted(word))].append(word)
    return list(anagrams.values())

```

단어들을 정렬하면 같은 값을 가지게 되므로
정렬된 단어를 키로 하여 리스트 값을 가지는 딕셔너리를 만든다.
그리고 딕셔너리의 값들을 리스트로 반환한다.
