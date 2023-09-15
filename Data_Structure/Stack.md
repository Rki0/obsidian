
- LIFO(Last In First Out) 원칙에 기반한 선형 자료구조

# Feature
- `Array`로 구현하느냐, `Linked List`로 구현하느냐에 따라 시간 복잡도가 달라진다.
- `DFS(Depth First Search)`에 주로 사용된다.

# Time Complexity
- Access : O(n) - 선형으로 순차적 접근을 하기 때문.
- Searching : O(n) - 선형으로 순차적 접근을 하기 때문.
- Insertion : O(1) - 항상 맨 뒤에 데이터를 넣기 때문.
- Removal
	- `Array`로 구현한 경우 : O(n) - 배열 특성 상, 하나의 인덱스가 제거되면 그 뒤의 모든 인덱스의 변경이 발생하기 때문.
	- `Linked List`로 구현한 경우 : O(1) - `head`를 통해 맨 위로 바로 접근하며, node의 참조만 변경하면 되기 때문.

# Stack Overflow / Underflow
## Overflow
- Stack이 할당된 메모리 용량을 초과하여 아이템을 추가하려고 하는 경우 발생
# Underflow
- Stack이 비어있을 때 아이템을 제거하려고 하는 경우 발생
## What is the problem?
- 이 현상들은 보통 클라이언트 요청을 처리하는 과정에서 발생한다.
- `Spring Boot`의 경우 내부적으로 여러 `Thread`를 사용하여 작업하는데, `Stack Overflow`는 `Thread` 별로 할당된 `Stack` 메모리 영역에서 발생한다. 보통 재귀 함수 호출이 너무 깊게 들어가거나, `Stack` 프레임의 크기가 너무 커져서 발생한다.
- `Main Thread(e.g. server.js, app.js)`에서 발생하는 경우에는 애플리케이션 전체가 시작되지 않거나 중단될 수 있다.
- 하나의 요청을 처리하는 `Thread`에서는 해당 `Thread`만 종료되고, 다른 `Thread`는 영향을 받지 않는다.
# How can we prevent?
- 재귀 함수의 깊이를 제한하거나, 재귀 대신 반복문을 사용하는 방법을 고민.
- 예외 처리를 통해 적절한 에러를 발생시키도록 한다. 단, `Main Thread`에서는 안하는게 좋은데, 이는 예외 처리할 문제가 아니라 파고들어서 해결해야하는 문제이기 때문에, 직접 문제를 파악하는 것이 좋다. 비즈니스 로직에 대해서는 적절한 에러를 발생시키도록 하고, 로그를 남기는 것이 좋을 것이다.

# How to reverse a string using Stack?
1. 빈 `Stack`을 생성한다.
2. 문자열을 `Stack`에 전부 `push`한다.
3. `Stack`을 전부 `pop`한다.

# 

# Implement
## Using Array

```js
let stack = [];
```
## Using [[Singly Linked List]]

```js

```

