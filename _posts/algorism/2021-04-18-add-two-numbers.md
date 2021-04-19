---
title: 알고리즘 - 역순 연결 리스트
date: 2021-04-18
category: 알고리즘
tags: 알고리즘
---

[리트코드 2번 문제](https://leetcode.com/problems/add-two-numbers/)

### 역순으로 저장된 연결 리스트의 숫자를 더하라.

| Input                                | Output            |
| ------------------------------------ | ----------------- |
| l1 = [2,4,3], l2 = [5,6,4]           | [7,0,8]           |
| l1 = [0], l2 = [0]                   | [0]               |
| l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9] | [8,9,9,9,0,0,0,1] |

<br><br>

## 처음 내가 한 풀이

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        def reverse(node: ListNode) -> ListNode:
                cur, prev = node, None
                while cur:
                    next, cur.next = cur.next, prev
                    cur, prev = next, cur
                return prev

        r1, r2 = reverse(l1), reverse(l2)
        r1_list, r2_list = [], []
        while r1:
            r1_list.append(str(r1.val))
            r1 = r1.next
        while r2:
            r2_list.append(str(r2.val))
            r2 = r2.next
        sum_result = int(''.join(r1_list)) + int(''.join(r2_list))
        sum_result_list = [int(i) for i in str(sum_result)]

        head = None
        for i in range(len(sum_result_list)):
            if len(sum_result_list) < 2:
                head = ListNode(sum_result_list[i])
            else:
                if i == 0 :
                    head = ListNode(sum_result_list[i], ListNode(sum_result_list[i + 1]))
                    cur = head
                else:
                    cur.next = ListNode(sum_result_list[i])
                    cur = cur.next

        return reverse(head)
```

자료형 변환을 통해 문제를 풀었다.  
그러나 정답을 맞추는데 급급하다보니 코드가 굉장히 지저분하다.

## Solution1 (코드 개선)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    # 연결 리스트 뒤집기
    def reverseList(self, head: ListNode) -> ListNode:
        node, prev = head, None

        while node:
            next, node.next = node.next, prev
            prev, node = node, next

        return prev


    # 연결 리스트를 파이썬 리스트로 변환
    def toList(self, node: ListNode) -> List:
        list = []
        while node:
            list.append(node.val)
            node = node.next
        return list

    def toReverseLinkedList(self, result: str) -> ListNode:
        prev: ListNode = None
        for r in result:
            node = ListNode(r)
            node.next = prev
            prev = node

        return node

    # 두 연결 리스트의 덧셈
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        a = self.toList(self.reverseList(l1))
        b = self.toList(self.reverseList(l2))

        resultStr = int(''.join(str(e) for e in a)) + \
                    int(''.join(str(e) for e in b))

        # 최종 계산 결과가 연결 리스트 변환
        return self.toReverseLinkedList(str(resultStr))
```

함수별로 역할 구분하여 코드가 구조적으로 잘 짜여져있다.  
문자열을 인자로 받아서 뒤집어진 연결 리스트를 만드는 코드가 이전보다 깔끔하고 간결하게 작성되었다.

<br><br>

## Solution2 (전가산기 구현)

```python
def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
    root = head = ListNode(0)

    carry = 0
    while l1 or l2 or carry:
        sum = 0
        # 두 입력값의 합 계산
        if l1:
            sum += l1.val
            l1 = l1.next
        if l2:
            sum += l2.val
            l2 = l2.next

        # 몫(자리올림수)과 나머지(값) 계산
        # divmod()함수는 몫과 나머지를 튜플로 반환해준다
        carry, val = divmod(sum + carry, 10)
        head.next = ListNode(val)
        head = head.next

    return root.next
```

바로 떠오르는 풀이는 아니지만 코드가 훨씬 깔끔하고 아름답다.
