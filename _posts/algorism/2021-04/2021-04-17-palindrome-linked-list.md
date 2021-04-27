---
title: 알고리즘 - 팰린드롬 연결 리스트
date: 2021-04-17
category: 알고리즘
tags: 알고리즘
---

[리트코드 234번 문제](https://leetcode.com/problems/palindrome-linked-list/)

### 연결 리스트가 팰린드롬 구조인지 판별하라.

| Input     | Output |
| --------- | ------ |
| [1,2,2,1] | true   |
| [1,2]     | false  |

<br><br>

## Solution1 (데크 사용)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        q = collections.deque()

        if not head:
            return True

        node = head
        while node is not None:
            q.append(node.val)
            node = node.next

        while len(q) > 1:
            if q.popleft() != q.pop():
                return False
        return True
```

list로도 풀이 가능하지만 deque를 사용하는 것이 최적화 측면에서 더 좋다.

## Solution2 (투 포인터 사용)

```python
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        q = []]

        if not head:
            return True

        node = head
        while node is not None:
            q.append(node.val)
            node = node.next

        left, right = 0, len(q) - 1

        while left < right:
            if q[left] != q[right]:
                return False
            left += 1
            right -= 1

        return True
```

리스트 보단 빠르고 데크보단 느리다.

## Solution3 (런너를 이용한 우아한 풀이)

```python
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        rev = None
        slow = fast = head
        # 런너를 이용해 역순 연결 리스트 구성
        while fast and fast.next:
            fast = fast.next.next
            rev, rev.next, slow = slow, rev, slow.next
        # 홀수와 짝수일때 slow값 처리 ( 홀수일 때 슬로우 한칸 더 이동)
        if fast:
            slow = slow.next

        while rev and rev.val == slow.val:
            slow, rev = slow.next, rev.next

        # 정상적으로 비교가 종료됬다면 slow or rev는 모두 끝까지 이동해 None이 됨
        return not rev
```

빠른 런너는 두 칸씩 건너뛰고 느린 런너는 한 칸씩 이동한다.  
빠른 런너가 연결리스트 끝에 도착하면 느린 런너는 연결 리스트의 중간 지점에 있다.  
느린 런너의 값과 느린 런너의 값을 뒤집은 값이 일치하는지 확인하는 알고리즘이다.  
실행시간은 가장 빠르다.

```python
# 1정답
rev, rev.next, slow = slow, rev, slow.next
# 2오답
rev, rev.next = slow, rev
slow = slow.next
"""
rev = 1, slow = 2->3 이라 가정
1. rev = 2->3, rev.next = 1, slow = 3
 따라서 rev = 2->1 slow = 3으로 최종 값이 결정된다.
 (한 번의 트랜잭션으로 처리)

2. rev = 2->3 rev.next = 1
   여기서 rev = 2->1 이 되었다.
   하지만 slow = rev 이기 때문에 slow의 값도 2->1로 되어버린다.
   즉, slow 와 rev가 동일 참조가 되어 slow의 값이 rev값과 동일해진다.
"""
파이썬의 = 연산자는 불변객체(str, int, ..)에 대해 값을 할당 하는 것이 아닌 참조하는 것
```
