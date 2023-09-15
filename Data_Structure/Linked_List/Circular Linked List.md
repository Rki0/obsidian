<img width="100%" alt="image" src="https://github.com/Rki0/obsidian/assets/86224851/522fc194-56c5-4863-b2e0-1a96f2c13095">

# Comparisons with other Linked List
- `Circular Linked List`는 일반적인 `Linked List`와 달리, 순환 구조를 가진다.
- 일반적으로는 `tail`이 `head`로 다시 이어지는 형태이며, 양 끝이 아닌 중간 `node`에서 순환이 발생할 수도 있다.
- [[Singly Linked List]], [[Doubly Linked List]] 중 어떤 것으로도 구현할 수 있다.

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

# Related Algorithm
- [[Floyd's Cycle Detection Algorithm(Tortoise and Hare Algorithm)]] : 순환을 검출하는데 사용되는 알고리즘

# Implement
- [[Singly Linked List]]를 사용하여 구현한 코드이다.
- `tail`이 `head`와 이어지도록 만들어야한다는 것과 일반적인 순회를 적용하면 무한 루프에 빠져버리는 것에 주의하자.
- `findCircular()`메서드의 경우, `tail`이 `head`와 연결되어 있다면 `tail`을 반환하지만, `tail`과 `head`가 아니라 다른 node끼리 순환인 경우에는 순환이 발생하는 node를 반환한다.

```js
class Node {
	constructor(val){
		this.val = val;
		this.next = null;
	}
}

class CircularLinkedList{
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
			newNode.next = this.head;
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
		let count = 0;

		while(count < this.length){
			newTail = current;
			current = current.next;
			count++;
		}

		this.tail = newTail;
		this.tail.next = this.head;
		this.length--;

		if(this.length === 0){
			this.head = null;
			this.tail = null;
		}

		current.next = null;

		return current;
	}

	shift(){
		if(!this.head){
			return undefined;
		}

		let currentHead = this.head;
		this.head = currentHead.next;
		this.tail.next = this.head;
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
			this.tail.next = this.head;
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
			return undefined;
		}

		if(index === 0){
			return this.shift();
		}

		if(index === this.length - 1){
			return this.pop();
		}

		let prevNode = this.get(index - 1);
		let removed = prevNode.next;

		prevNode.next = removed.next;
		removed.next = null;

		this.length--;

		return removed;
	}

	findCircular(){
		if(!this.head){
			return undefined;
		}

		let rabbit = this.head.next;
		let turtle = this.head;

		while(rabbit){
			if(rabbit === turtle){
				return rabbit;
			}

			if(rabbit.next){
				rabbit = rabbit.next.next;
				turtle = turtle.next;
				continue;
			}

			break;
		}
	}
}
```

