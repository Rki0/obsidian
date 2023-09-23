# Feature
- 각각의 노드가 우선 순위(priority)를 가진다.
- 우선 순위가 높은 노드가 우선 순위가 낮은 노드보다 먼저 처리된다.
- 보통 [[Heap(Binary Heap)]]을 사용하여 구현한다.

# Implement
```js
class Node{
	constructor(val, priority){
		this.val = val;
		this.priority = priority;
	}
}

class PriorityQueue{
	constructor(){
		this.values = [];
	}

	enqueue(val, priority){
		let newNode = new Node(val, priority);

		this.values.push(newNode);
		
		this.bubbleUp();
	}

	bubbleUp(){
		let index = this.values.l
	}
}
```