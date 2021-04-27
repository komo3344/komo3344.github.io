---
title: 알고리즘 - 역순 연결 리스트
date: 2021-04-17
category: 알고리즘
tags: 알고리즘
---

[리트코드 206번 문제](https://leetcode.com/problems/reverse-linked-list/)

### 연결 리스트를 뒤집어라.

| Input       | Output      |
| ----------- | ----------- |
| [1,2,3,4,5] | [5,4,3,2,1] |

<br><br>

## Solution1 (반복문)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        node, prev = head, None

        while node:
            # next는 노드의 다음 값을 저장하는 변수, node.next는 이전 값을 가리키도록 함
            next, node.next = node.next, prev
            # 현재 노드가 이전 노드로 연결되었으므로
            # 이전 노드가 현재 노드를 가리키도록 하고 현재 노드는 다음 노드를 가리키도록 포인트를 이동시킴
            prev, node = node, next
        return prev

```

## Solution2 (재귀)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        def reverse(node: ListNode, prev: ListNode = None):
            if node is None:
                return prev
            next, node.next = node.next, prev
            return reverse(next, node)

        return reverse(head)
```

다음 노드와 현재 노드를 파라미터로 지정함 함수를 계속 재귀 호출한다.  
node.next에는 이전 prev리스트를 계속 연결해주면서 node가 None이 될 때까지 재귀호출한다.
<br><br>
반복을 통한 풀이가 재귀에 비해 공간 복잡도는 낮은편이며 속도도 더 빠른 편이다.
