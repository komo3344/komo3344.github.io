---
title: 알고리즘 - 역순 연결 리스트2
date: 2021-04-18
category: 알고리즘
tags: 알고리즘
---

[리트코드 92번 문제](https://leetcode.com/problems/reverse-linked-list-ii/)

### 인덱스 left에서 right까지를 역순으로 만들어라. 인덱스 left는 1부터 시작한다.

| Input                                   | Output      |
| --------------------------------------- | ----------- |
| head = [5], left = 1, right = 1         | 5           |
| head = [1,2,3,4,5], left = 2, right = 4 | [1,4,3,2,5] |

<br><br>

## Solution1 (반복 구조로 노드 뒤집기)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseBetween(self, head: ListNode, left: int, right: int) -> ListNode:
        if not head or left == right:
            return head

        root = start = ListNode(None)
        root.next = head

        for _ in range(left - 1):
            start = start.next
        end = start.next

        for _ in range(right - left):
            tmp, start.next, end.next = start.next, end.next, end.next.next
            start.next.next = tmp
        return root.next

```

start는 변경이 필요한 지점 바로 앞, end는 변경이 필요한 시점으로 끝까지 값이 변경되지 않는다.  
start,end 변수지정과 right-left만큼 반복하며 뒤집는 것이 핵심이다.  
연결 리스트 문제는 포인터들을 자유자재로 옮길 수 있도록 변수 지정을 잘 하는 것이 중요하다.
