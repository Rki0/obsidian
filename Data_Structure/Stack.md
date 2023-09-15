
- LIFO(Last In First Out) 원칙에 기반한 선형 자료구조

# Feature
- `Array`로 구현하느냐, `Linked List`로 구현하느냐에 따라 시간 복잡도가 달라진다.
- `DFS(Depth First Search)`에 주로 사용된다.

# Stack Overflow / Underflow
## Overflow
- Stack이 할당된 메모리 용량을 초과하여 아이템을 추가하려고 하는 경우 발생
# Underflow
- Stack이 비어있을 때 아이템을 제거하려고 하는 경우 발생
## What is the problem?
- 이 현상들은 보통 클라이언트 요청을 처리하는 과정에서 발생한다.
- Spring Boot의 경우 내부적으로 여러 Thread를 사용하여 작업하는데, `Stack Overflow`는 Thread 별로 할당된 Stack 메모리

# Time Complexity
- Access : O(n) - 선형으로 순차적 접근을 하기 때문.
- Searching : O(n) - 선형으로 순차적 접근을 하기 때문.
- Insertion : O(1) - 항상 맨 뒤에 데이터를 넣기 때문.
- Removal
	- `Array`로 구현한 경우 : O(n) - 배열 특성 상, 하나의 인덱스가 제거되면 그 뒤의 모든 인덱스의 변경이 발생하기 때문.
	- `Linked List`로 구현한 경우 : O(1) - `head`를 통해 맨 위로 바로 접근하며, node의 참조만 변경하면 되기 때문.

# Implement
## Using Array

```js
let stack = [];
```
## Using [[Singly Linked List]]

```js

```

