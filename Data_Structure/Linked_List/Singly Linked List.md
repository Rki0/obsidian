<img width="100%" alt="스크린샷 2023-09-13 오후 1 37 07" src="https://github.com/Rki0/obsidian/assets/86224851/68a8ae9e-2785-4ff3-b7ba-1b2033313eab">

# Time Complexity
- Access : O(n) - 순차적으로 node를 따라가기 때문.
- Searching : O(n) - 순차적으로 node를 따라가기 때문.
- Insertion
	- 맨 앞에 삽입 : O(1) - 가장 앞의 node에 연결하기 때문에, head의 참조만 바꾸면 된다.
	- 중간, 맨 끝에 삽입 : O(n) - 삽입할 위치를 찾아야하기 때문에.
	- tail을 사용하여 맨 끝에 삽입 : O(1) - tail을 통해 마지막 node의 참조를 바꾸면 된다.
- Removal
	- 맨 앞을 제거 : O(1) - 가장 앞의 node를 제거하기 때문에, head의 참조만 바꾸면 된다.
	- 중간, 맨 끝을 제거 : O(n) - 삭제하기 위한 node를 찾아야하기 때문에.

# Implement
```js
class Node {
	constructor(val){
		this.val = val;
		this.next = null;
	}
}

class SinglyLinkedList{
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
			this.tail = newNode;
		}

		this.length++;

		return this;
	}

	pop(){
		if(!this.head){
			return undefined;
		}

		let current = this.head;
		let newTail = current;

		while(current.next){
			newTail = current;
			current = current.next;
		}

		this.tail = newTail;
		this.tail.next = null;
		this.length--;

		if(this.length === 0){
			this.head = null;
			this.tail = null;
		}

		return current;
	}

	shift(){
		if(!this.head){
			return undefined;
		}

		let currentHead = this.head;
		this.head = currentHead.next;
		this.length--;

		if(this.length === 0){
			this.tail = null;
		}

		currentHead.next = null;

		return currentHead;
	}

	unshift(val){
		const newNode = new Node(val);

		if(!this.head){
			this.head = newNode;
			this.tail = newNode;
		} else {
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

		if(!foundNode){
			return false;
		}
		
		foundNode.val = val;
		return true;
	}

	insert(index, val){
		if(index < 0 || index > this.length){
			return false;
		}

		if(index === 0){
			return !!this.unshift(val);
		}

		if(index === this.length){
			return !!this.push(val);
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

	reverse(){
		let node = this.head;
		this.head = this.tail;
		this.tail = node;

		let prev = null;
		let next = null;

		for(let i = 0; i < this.length; i++){
			next = node.next;
			node.next = prev;
			prev = node;
			node = next;
		}

		return this;
	}
}
```
