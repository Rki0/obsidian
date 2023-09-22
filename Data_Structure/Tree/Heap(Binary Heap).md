# Feature
- 보통 `Heap`이라고 하면, `Binary Heap`을 말한다.
- 완전 이진 트리 형태여야한다. === 부모가 최대 2개의 자식 노드를 가진다.
- 모든 부모 노드가 자식 노드보다 크거나 같은 값을 가져야한다. === `Max Heap`
- 모든 부모 노드가 자식 노드보다 작거나 같은 값을 가져야한다. === `Min Heap`
- 형제 노드 간의 관계에 대해서는 조건이 없다는 것에 주의!

# Complete Binary Tree
- 완전 이진 트리의 특성 상 각 노드의 위치를 배열의 인덱스로 쉽게 표현할 수 있다!
- 따라서, 노드 간의 관계를 인덱스를 통해 쉽게 파악하고 조작할 수 있다.

```
arr[i]를 기준으로

- 왼쪽 자식 노드 : arr[2i + 1]
- 오른쪽 자식 노드 : arr[2i + 2]
- 부모 노드 : arr[(i - 1) / 2]

ex) Max Heap [100, 70, 50, 35, 25, 45, 15]

부모 노드(0번 인덱스) : 100
왼쪽 자식 노드(1) : 70
오른쪽 자식 노드(2) : 50

부모 노드(1번 인덱스) : 70
왼쪽 자식 노드(3) : 35
오른쪽 자식 노드(4) : 25

부모 노드(2번 인덱스) : 50
왼쪽 자식 노드(5) : 45
오른쪽 자식 노드(6) : 15
```

# Priority Queue
- `Heap`은 `Priority Queue`를 구현하는데 사용된다.
- `Priority Queue`에서는 검색 기능은 필요없고, 우선 순위가 가장 높은 순서대로 반환하고 삭제할 수 있으면 된다!

# Time Complexity
Insertion : O(log n)
Removal : O(log n)
Search : O(n) => 탐색을 중점적으로 하려면 `Heap`보다는 `BST`쪽이 더 활용도가 높다.

# Pseudo Code


# Implement

```js
class MaxBinaryHeap{
	constructor(){
		this.values = [];
	}
}
```
