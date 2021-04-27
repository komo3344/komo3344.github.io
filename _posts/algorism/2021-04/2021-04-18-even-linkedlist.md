---
title: 알고리즘 - 홀짝 연결 리스트
date: 2021-04-18
category: 알고리즘
tags: 알고리즘
---

[리트코드 328번 문제](https://leetcode.com/problems/odd-even-linked-list/)

### 연결 리스트를 홀수 노드 다음에 짝수 노드가 오도록 재구성하라. 공간 복잡도 O(1), 시간 복잡도 O(n)에 풀이하라.

| Input                  | Output          |
| ---------------------- | --------------- |
| head = [1,2,3,4,5]     | [1,3,5,2,4]     |
| head = [2,1,3,5,6,4,7] | [2,3,6,7,1,5,4] |

<br><br>

## Solution1 (반복 구조로 홀짝 노드 처리)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def oddEvenList(self, head: ListNode) -> ListNode:
        """
        홀수 노드끼리 연결, 짝수 노드끼리 연결 후
        홀수노드 마지막 포인터를 짝수 노드 처음과 연결해준다.
        """
        # 예외처리
        if head is None:
            return None

        odd = head
        even = head.next
        even_head = head.next

        while even and even.next:
            odd.next, even.next = odd.next.next, even.next.next
            odd, even = odd.next, even.next

        odd.next = even_head
        return head
```

공간 복잡도와 시간복잡도의 제약 사항이 없다면  
연결리스트를 리스트로 바꾸고 파이썬 리스트가 제공하는 슬라이싱과 같은 다양한 함수를 통해 쉽고 직관적이며 빠르게 풀 수 있다.

파이썬으로 직접 코딩한 알고리즘의 실행 속도보다 C로 구현된 파이썬 내장 함수의 실행 속도가 훨씬 더 빠르기 때문이다.
