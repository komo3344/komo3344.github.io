---
title: 알고리즘 - K개 정렬 리스트 병합
date: 2021-04-21
category: 알고리즘
tags: 알고리즘
---

[리트코드 23번 문제](https://leetcode.com/problems/merge-k-sorted-lists/)

### k개의 정렬된 리스트를 1개의 정렬된 리스트로 병합하라.

| Input                           | Output            |
| ------------------------------- | ----------------- |
| lists = [[1,4,5],[1,3,4],[2,6]] | [1,1,2,3,4,4,5,6] |

## Solution (우선순위 큐를 이용한 리스트 병합)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        root = current = ListNode(None)
        data = []

        for node in lists:
            # heap 최소값을 보장해주는 자료형
            # 각각의 k개의 노드들을 순회 하면서 data에 값을 저장
            while node:
                heapq.heappush(data, node.val)
                node = node.next

        while data:
            val = heapq.heappop(data)
            current.next = ListNode(val)
            current = current.next

        return root.next
```
