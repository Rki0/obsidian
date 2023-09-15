
# Feature
- FIFO(First In First Out) 원칙에 기반한 선형 자료구조.
- 삽입, 삭제를 최우선으로 하는 자료구조이므로 `Searching`이 중요하다면 일반 배열이나 다른 구조를 사용하자.
- 주문 처리 시스템, 스트리밍(받아온 데이터 패킷을 순차적으로 합치는 기능), Load Balancer 등에 사용
- `BFS(Breadth First Search)` 구현에 주로 사용된다.
- `Linear Queue(선형 큐)`, `Circular Queue(환형 큐)`, `Priority Queue(우선 순위 큐)`, `Double-ended Queue(Dequeue, 데크)` 등이 있다.
- `Linear Queue`의 경우 사용하지 못하는 공간이 발생하기 때문에, 보통 `Circular Queue`를 사용한다.
- `Circular Queue`는 `front`, `rear` 포인터를 활용하여 배열의 마지막 요소 다음에에는 배열의 첫 번째 요소가 되도록 구현하며, 공간을 재사용할 수 있다는 장점이 있다.

# Time Complexity
- Access : O(n) - 선형으로 순차적 접근을 하기 때문.
- Searching : O(n) - 선형으로 순차적 접근을 하기 때문.
- Insertion : O(1) - 항상 맨 뒤에 데이터를 넣기 때문.
- Removal : O(1) - 항상 맨 앞에서 데이터를 꺼내기 때문.

# What should I use for implementation? Array or Linked List?
1. Array로 구현하는 방법
	- `push, shift` 메서드를 사용하거나, `unshift`
1. Linked List로 구현하는 방법

# Implement

```js

```
