---
title: 알고리즘 - 원형 큐 디자인
date: 2021-04-20
category: 알고리즘
tags: 알고리즘
---

[리트코드 622번 문제](https://leetcode.com/problems/design-circular-queue/)

### 원형 큐를 디자인하라.

MyCircularQueue클래스 구현 :

- **MyCircularQueue**(k): 큐의 크기가 k인 개체를 초기화합니다.
- **int Front()**: 큐에서 앞 항목을 가져옵니다. 대기열이 비어 있으면 -1을 반환합니다.
- **int Rear()**: 큐에서 마지막 항목을 가져옵니다. 대기열이 비어 있으면 -1을 반환합니다.
- **boolean enQueue(int value)**: 순환 대기열에 요소를 삽입합니다. 작업이 성공하면 true 반환합니다.
- **boolean deQueue()**: 순환 대기열에서 요소를 삭제합니다. 작업이 성공하면 true 반환합니다.
- **boolean isEmpty()**: 순환 대기열이 비어 있는지 여부를 확인합니다.
- **boolean isFull()** 순환 대기열이 가득 찼는지 여부를 확인합니다.

```python
class MyCircularQueue:

    def __init__(self, k: int):
        # 초기화 시에는 큐의 크기 k를 입력으로 받는다.
        # k값은 최대 길이가 되고 front 포인터는 p1, rear 포인터는 p2로 하고 0으로 초기화
        self.q = [None] * k
        self.maxlen = k
        self.p1 = 0
        self.p2 = 0

    def enQueue(self, value: int) -> bool:
        # rear 값이 비어있다면 값을 넣고 포인터를 앞으로 한칸 움직인다.
        # 포인터의 위치가 전체 길이를 벗어나지 않게 하기 위해 전체 길이만큼 나머지 연산을 한다.
        if self.q[self.p2] is None:
            self.q[self.p2] = value
            self.p2 = (self.p2 + 1) % self.maxlen
            return True

        # rear 포인터 위치가 None이 아니라면 다른 요소가 있는 공간이 꽉 찬 상태이거나 비정상적인 경우
        else:
            return False

    def deQueue(self) -> bool:
        if self.q[self.p1] is None:
            return False

        # front 포인터인 p1에 None을 넣어 삭제하고 포인트를 앞으로 이동시킨다.
        else:
            self.q[self.p1] = None
            self.p1 = (self.p1 + 1) % self.maxlen
            return True

    def Front(self) -> int:
        return -1 if self.q[self.p1] is None  else self.q[self.p1]

    def Rear(self) -> int:
        return -1 if self.q[self.p2 - 1] is None else self.q[self.p2 - 1]

    def isEmpty(self) -> bool:
        return self.p1 == self.p2 and self.q[self.p1] is None

    def isFull(self) -> bool:
        return self.p1 == self.p2 and self.q[self.p1] is not None


# Your MyCircularQueue object will be instantiated and called as such:
# obj = MyCircularQueue(k)
# param_1 = obj.enQueue(value)
# param_2 = obj.deQueue()
# param_3 = obj.Front()
# param_4 = obj.Rear()
# param_5 = obj.isEmpty()
# param_6 = obj.isFull()
```
