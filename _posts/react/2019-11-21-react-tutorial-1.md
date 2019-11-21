---
title: React 튜토리얼
date: 2019-11-21
category: react
---
[튜토리얼 따라하기]

진행하며 알게된 점 정리

# component들간의 관계를 형성하여 데이터를 주고 받을 수 있다.


  예를들어

  class squire 은 3x3 각각의 버튼 1개에 대한 컴포넌트이고

  class board 는 버튼의 선택 정보를 가지고 있는 컴포넌트이다.


  board에서 각 버튼의 값을 저장하는 state와 onClick()(버튼 클릭시 이벤트 처리 함수)를 squire에게 넘겨주고

  squire에서는 board에게 넘겨받은 값들을 props를 통해 받아 사용할 수 있다.


# 원본데이터를 건드리지 않고 복사본을 이용하는 이유에 대해서 알게되었다.

  데이터의 값을 변경하는 방법은 2가지가 있는데 첫 번째는 원본데이터의 값을 변경하는 것이고,

  두 번째는 복사본을 생성하여 원본은 놔두고 복사본 데이터로 처리하는 방법이 있다.



  상황에 따라 다르겠지만 복사본을 생성하여 데이터 처리할 때의 이점은 3가지가 있다고 한다.

  1. 이전으로 되돌릴 수 있다.

  2. 변화를 감지 할 수 있다.

  3. react에서 렌더링하는 시기를 결정할 수 있다.
















[튜토리얼 따라하기]: https://ko.reactjs.org/tutorial/tutorial.html#setup-option-2-local-development-environment
