

# Comparisons with [[Singly Linked List]]
- 이전 노드를 저장해야하기 때문에 메모리를 더 많이 사용한다.
- 그 대신 Searching은 [[Singly Linked List]]보다 절반의 시간만 걸린다.
- prev를 통해 이전 node의 접근할 수 있기 때문에, 마지막 node를 삭제하는 것이 O(1)의 시간을 가진다.

# Time Complexity
- Access : O(n) - 순차적으로 node를 따라가기 때문.
- Searching : O(n) - 순차적으로 node를 따라가기 때문. 정확히는 `(1/2) * n`이지만 n으로 표기한다.
- Insertion
	- 맨 앞에 삽입 : O(1) - 가장 앞의 node에 연결하기 때문에, head의 참조만 바꾸면 된다.
	- 중간, 맨 끝에 삽입 : O(n) - 삽입할 위치를 찾아야하기 때문에.
	- tail을 사용하여 맨 끝에 삽입 : O(1) - tail을 통해 마지막 node의 참조를 바꾸면 된다.
- Removal
	- 맨 앞을 제거 : O(1) - 가장 앞의 node를 제거하기 때문에, head의 참조만 바꾸면 된다.
	- 중간, 맨 끝을 제거 : O(n) - 삭제하기 위한 node를 찾아야하기 때문에.
	- tail을 사용하여 맨 끝을 제거 : O(1) - tail을 통해 마지막 node의 이전 node에 접근할 수 있기 때문.

# Implement
```js
class Node {
	constructor(val){
		this.val = val;
		this.next = null;
		this.prev = null;
	}
}

class DoublyLinkedList {
	constructor(){
		this.head = null;
		this.tail = null;
		this.length = 0;
	}

	push(val){
		const newNode = new Node(val);

		if(!this.head){
			this.head = newNode;
			this.tail = newNode;
		} else {
			this.tail.next = newNode;
			newNode.prev = this.tail;
			this.tail = newNode;
		}

		this.length++;

		return this;
	}

	pop(){
		if(!this.head){
			return undefined;
		}

		const popedNode = this.tail;

		if(this.length === 1){
			this.head = null;
			this.tail = null;
		} else {
			this.tail = popedNode.prev;
			this.tail.next = null;
			popedNode.prev = null;
		}

		this.length--;

		return popedNode;
	}

	shift(){
		if(!this.head){
			return undefined;
		}

		let oldHead = this.head;

		if(this.length === 1){
			this.head = null;
			this.tail = null;
		} else {
			this.head = oldHead.next;
			this.head.prev = null;
			oldHead.next = null;
		}

		this.length--;

		return oldHead;
	}

	unshift(val){
		const newNode = new Node(val);

		if(!this.head){
			this.head = newNode;
			this.tail = newNode;
		} else {
			this.head.prev = newNode;
			newNode.next = this.head;
			this.head = newNode;
		}

		this.length++;

		return this;
	}

	get(index){
		if(index < 0 || index >= this.length){
			return null;
		}

		let current = this.head;
		let count = 0;

		while(count < index){
			current = current.next;
			count++;
		}

		return current;
	}

	set(index, val){
		const foundNode = this.get(index);

		if(foundNode){
			foundNode.val = val;
			return true;
		}

		return false;
	}

	insert(index, val){
		if(index < 0 || index >= this.length){
			return false;
		}

		if(index === this.length){
			return !!this.push(val);
		}

		if(index === 0){
			return !!this.unshift(val);
		}

		let prevNode = this.get(index - 1);
		let newNode = new Node(val);

		let temp = prevNode.next;
		prevNode.next = newNode;
		newNode.next = temp;

		this.length++;

		return true;
	}

	remove(index){
		if(index < 0 || index >= this.length){
			return false;
		}

		if(index === this.length - 1){
			return !!this.pop();
		}

		if(index === 0){
			return !!this.shift();
		}

		let prevNode = this.get(index - 1);
		let removed = prevNode.next;

		prevNode.next = removed.next;
		removed.next = null;

		this.length--;

		return removed;
	}
}
```