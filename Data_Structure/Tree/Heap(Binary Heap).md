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
## insert
1. Push the value into the values property on the heap
2. Bubble the value up to its correct spot!
## bubbleUp
1. Create a variable called `index` which is the length of the values `property - 1`
2. Create a variable called parentIndex which is the floor of `(index - 1) / 2`
3. Keep looping as long as the values element at parentIndex is less than the values element at the child index
	1. Swap the value of the values element at the parentIndex with the value of the element property at the child index
	2. Set the index to be the parentIndex, and start over!
## extractMax
1. Swap the first value in the values property with the last one
2. Pop from 

# Implement

```js
class MaxBinaryHeap{
	constructor(){
		this.values = [];
	}

	insert(element){
		this.values.push(element);

		this.bubbleUp();
	}

	bubbleUp(){
		let index = this.values.length - 1;
		const element = this.values[index];

		while(index > 0){
			let parentIndex = Math.floor((index - 1) / 2);
			let parent = this.values[parentIndex];

			if(element <= parent){
				break;
			}

			this.values[parentIndex] = element;
			this.values[index] = parent;

			index = parentIndex;
		}
	}

	extractMax(){
		const max = this.values[0];
		const end = this.values.pop();

		if(this.values.length > 0){
			this.values[0] = end;

			this.sinkDown();
		}

		return max;
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
				letfChild = this.values[leftChildIndex];

				if(leftChild > element){
					swap = leftChildIndex;
				}
			}

			if(rightChildIndex < this.values.length){
				rightChild = this.values[rightChildIndex];

				if(
					(swap === null && rightChild > element) ||
					(swap !== null && rightChild > leftChild)
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
