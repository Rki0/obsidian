<img width="100%" alt="image" src="https://www.techiedelight.com/wp-content/uploads/2016/11/Min-Heap.png">
image source : https://www.techiedelight.com/introduction-priority-queues-using-binary-heaps/
# Feature
- 각각의 노드가 우선 순위(priority)를 가진다.
- 우선 순위가 높은 노드가 우선 순위가 낮은 노드보다 먼저 처리된다.
- 보통 [[Heap(Binary Heap)]]을 사용하여 구현한다.

# Implement
- `Min Heap`으로 구현되었다. 루트 노드에 가까울 수록 우선 순위가 높다고 생각하면 된다.
- 즉, `priority`가 1에 가까울수록 우선 처리 되어야하는 노드이다.
- `priority` 외에도 다른 값들을 사용해서 다양한 조건으로 활용할 수 있다.

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
		let index = this.values.length - 1;
		const element = this.values[index];

		while(index > 0){
			let parentIndex = Math.floor((index - 1) / 2);
			let parent = this.values[parentIndex];

			if(element.priority >= parent.priority){
				break;
			}

			this.values[parentIndex] = element;
			this.values[index] = parent;

			index = parentIndex;
		}
	}

	dequeue(){
		const min = this.values[0];
		const end = this.values.pop();

		if(this.values.length > 0){
			this.values[0] = end;

			this.sinkDown();
		}

		return min;
	}

	sinkDown(){
		let index = 0;
		const element = this.values[0];

		while(true){
			let leftChildIndex = 2 * index + 1;
			let rightChildIndex = 2 * index + 2;

			let leftChild = null;
			let rightChild = null;
			let swap = null;

			if(leftChildIndex < this.values.length){
				leftChild = this.values[leftChildIndex];

				if(leftChild.priority < element.priority){
					swap = leftChildIndex;
				}
			}

			if(rightChildIndex < this.values.length){
				rightChild = this.values[rightChildIndex];

				if(
					(swap === null && rightChild.priority > element.priority) ||
					(swap !== null && rightChild.priority > leftChild.priority)
				){
					swap = rightChildIndex;
				}
			}

			if(swap === null){
				break;
			}

			this.values[index] = this.values[swap];
			this.values[swap] = element;

			index = swap;
		}
	}
}
```