---
title: 알고리즘 - 두 정렬 리스트의 병합
date: 2021-04-17
category: 알고리즘
tags: 알고리즘
---

[리트코드 21번 문제](https://leetcode.com/problems/merge-two-sorted-lists/)

### 정렬되어 있는 두 연결 리스트를 합쳐라.

| Input                      | Output        |
| -------------------------- | ------------- |
| l1 = [1,2,4], l2 = [1,3,4] | [1,1,2,3,4,4] |

<br><br>

## Solution (재귀 구조로 연결)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        if (not l1) or (l2 and l1.val > l2.val):
            l1, l2 = l2, l1

        if l1:
            l1.next = self.mergeTwoLists(l1.next, l2)

        return l1
```

| L1                   | L2          |
| -------------------- | ----------- |
| **1**->2->4          | **1**->3->4 |
| 1->**1**->3->4       | **2**->4    |
| 1->1->**2**->4       | **3**->4    |
| 1->1->2->**3**->4    | **4**       |
| 1->2->2->3->**4**    | **4**       |
| 1->2->2->3->4->**4** |             |
