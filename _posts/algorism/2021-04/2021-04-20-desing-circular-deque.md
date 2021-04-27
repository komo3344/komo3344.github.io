---
title: 알고리즘 - 원형 데크 디자인
date: 2021-04-20
category: 알고리즘
tags: 알고리즘
---

[리트코드 641번 문제](https://leetcode.com/problems/design-circular-deque/)

### 다음 연산을 제공하는 원형 데크를 디자인하라.

- **MyCircularDeque(k)**: 생성자, deque의 크기를 k로 설정합니다.
- **insertFront()**: Deque 앞에 아이템을 추가합니다. 작업이 성공하면 true를 반환합니다.
- **insertLast()**: Deque 뒤쪽에 아이템을 추가합니다. 작업이 성공하면 true를 반환합니다.
- **deleteFront()**: Deque 전면에서 항목을 삭제합니다. 작업이 성공하면 true를 반환합니다.
- **deleteLast()**: Deque 뒷면에서 항목을 삭제합니다. 작업이 성공하면 true를 반환합니다.
- **getFront()**: Deque에서 전면 항목을 가져옵니다. deque가 비어 있으면 -1을 반환합니다.
- **getRear()**: Deque에서 마지막 항목을 가져옵니다. deque가 비어 있으면 -1을 반환합니다.
- **isEmpty()**: Deque가 비어 있는지 확인합니다.
- **isFull()**: Deque가 가득 찼는 지 확인합니다.

```python
class MyCircularDeque:

    def __init__(self, k: int):
        """
        Initialize your data structure here. Set the size of the deque to be k.
        """
        self.head, self.tail = ListNode(None), ListNode(None)
        self.k, self.len = k, 0
        self.head.right, self.tail.left = self.tail, self.head


    def _add(self, node: ListNode, new: ListNode):
        n = node.right
        node.right = new
        new.left, new.right = node, n
        n.left = new

    def _del(self, node:ListNode):
        n = node.right.right
        node.right = n
        n.left = node

    def insertFront(self, value: int) -> bool:
        """
        Adds an item at the front of Deque. Return true if the operation is successful.
        """
        if self.len == self.k:
            return False
        self.len += 1
        self._add(self.head, ListNode(value))
        return True

    def insertLast(self, value: int) -> bool:
        """
        Adds an item at the rear of Deque. Return true if the operation is successful.
        """
        if self.len == self.k:
            return False
        self.len += 1
        self._add(self.tail.left, ListNode(value))
        return True

    def deleteFront(self) -> bool:
        """
        Deletes an item from the front of Deque. Return true if the operation is successful.
        """
        if self.len == 0:
            return False
        self.len -= 1
        self._del(self.head)
        return True

    def deleteLast(self) -> bool:
        """
        Deletes an item from the rear of Deque. Return true if the operation is successful.
        """
        if self.len == 0:
            return False
        self.len -= 1
        self._del(self.tail.left.left)
        return True

    def getFront(self) -> int:
        """
        Get the front item from the deque.
        """
        return self.head.right.val if self.len else -1

    def getRear(self) -> int:
        """
        Get the last item from the deque.
        """
        return self.tail.left.val if self.len else -1

    def isEmpty(self) -> bool:
        """
        Checks whether the circular deque is empty or not.
        """
        return self.len == 0

    def isFull(self) -> bool:
        """
        Checks whether the circular deque is full or not.
        """
        return self.len == self.k


# Your MyCircularDeque object will be instantiated and called as such:
# obj = MyCircularDeque(k)
# param_1 = obj.insertFront(value)
# param_2 = obj.insertLast(value)
# param_3 = obj.deleteFront()
# param_4 = obj.deleteLast()
# param_5 = obj.getFront()
# param_6 = obj.getRear()
# param_7 = obj.isEmpty()
# param_8 = obj.isFull()
```
