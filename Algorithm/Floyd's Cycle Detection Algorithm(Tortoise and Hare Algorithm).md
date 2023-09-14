`Floyd's Cycle Detection(Finding) Algorithm`이라고 불리는 이 것은 [[Linked List]]에서 순환(cycle)이 발생하는지 검출하는 알고리즘이다.

# Principle
원리는 다음과 같다.

- 두 개의 포인터를 사용하여, `pointer1`은 맨 앞 node에서, `pointer2`는 맨 앞의 다음 node에서 시작하게 한다.
- `pointer1`은 한 칸씩 움직이며, `pointer2`는 두 칸씩 움직인다.
- `pointer1`과 `pointer2`가 만날 때까지 이를 반복한다.
- `pointer1`과 `pointer2`가 만났다면, 바로 그 node가 순회가 발생하는 곳이다.

# Implement
`Linked List`에서 순환을 검출하는 것이므로, [[Singly Linked List]]에서 구현한 것을 활용하고자 한다.

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