---
title: 알고리즘 - 해시맵 디자인
date: 2021-04-21
category: 알고리즘
tags: 알고리즘
---

[리트코드 706번 문제](https://leetcode.com/problems/design-hashmap/)

### 다음 기능을 제공하는 해시맵을 디자인하라.

- put(key, value): 키, 값을 해시맵에 삽입한다. 만약 이미 존재하는 키라면 업데이트한다.
- get(key): 키에 해당하는 값을 조회한다. 만약 키가 존재하지 않는다면 -1을 리턴한다.
- remove(key): 키에 해당하는 키, 값을 해시맵에서 삭제한다.

<br><br>

## Solution (개별 체이닝 방식)

```python
class ListNode:
    def __init__(self, key=None, value=None):
        self.key = key
        self.value = value
        self.next = None

class MyHashMap:

    def __init__(self):
        self.size = 1000
        self.table = collections.defaultdict(ListNode)


    def put(self, key: int, value: int) -> None:
        # 문제에서 편의상 모든 키를 정수형으로 지정
        # size 개수만큼 나머지 연산을 한 값을 해시값으로 정함
        # 나머지 연산은 해시 테이블의 가장 기본적인 해싱방식
        index = key % self.size
        # 인덱스에 노드가 없다면 삽입 후 종료
        # defaultdict은 존재하지 않는 인덱스 접근시 디폴트 객체를 생성하기 때문에
        # table[index].value 로 검사한다. (table[index] is None 은 안됨)
        if self.table[index].value is None:
            self.table[index] = ListNode(key, value)
            return

        # 인덱스에 노드가 존재하는 경우 연결 리스트 처리(충돌난 경우)
        p = self.table[index]
        while p:
            # 키가 존재할 경우 값을 업데이트하고 나감
            if p.key == key:
                p.value = value
                return
            # p.next가 None이면 아무것도 하지 않고 빠져나간다.
            # 이것을 처리하지 않으면 p = p.next로 인해 p는 None이 되기 때문에
            # p.next = ListNode(key, value) 여기서 에러가 발생한다.
            if p.next is None:
                break
            p = p.next
        p.next = ListNode(key, value)

    def get(self, key: int) -> int:
        index = key % self.size
        if self.table[index].value == None:
            return -1

        # 노드가 존재할 때 일치하는 키 탐색
        p = self.table[index]
        while p:
            if p.key == key:
                return p.value
            p = p.next
        return -1

    def remove(self, key: int) -> None:
        index = key % self.size
        if self.table[index].value is None:
            return

        # 인덱스의 첫 번째 노드일 때 삭제처리
        p = self.table[index]
        if p.key == key:
            self.table[index] = ListNode() if p.next is None else p.next
            return

        # 연결 리스트 노드 삭제
        prev = p
        while p:
            if p.key == key:
                prev.next = p.next
                return
            prev, p = p, p.next

```
