
- LIFO(Last In First Out) 원칙에 기반한 선형 자료구조

# Feature
- `Array`로 구현하느냐, `Linked List`로 구현하느냐에 따라 시간 복잡도가 달라진다.(일반적인 경우)
- 어느 쪽으로 구현하든 같은 시간 복잡도를 가지게 하려면 `Linked List`를 수정해서 사용해야한다.
- 삽입, 삭제를 최우선으로 하는 자료구조라서 `Searching`이 중요하다면 일반 배열이나 다른 자료 구조를 쓰는게 좋다.
- `Managing function invocations`, `Undo/Redo`, `Routing(the history object)` 등에 사용된다.
- `DFS(Depth First Search)` 구현에 주로 사용된다.

# Time Complexity
- Access : O(n) - 선형으로 순차적 접근을 하기 때문.
- Searching : O(n) - 선형으로 순차적 접근을 하기 때문.
- Insertion : O(1) - 항상 맨 위에 데이터를 넣기 때문.
- Removal : O(1) - 항상 맨 위에서 데이터를 꺼내기 때문.

# Stack Overflow / Underflow
## Overflow
- Stack이 할당된 메모리 용량을 초과하여 아이템을 추가하려고 하는 경우 발생
# Underflow
- Stack이 비어있을 때 아이템을 제거하려고 하는 경우 발생
## What is the problem?
- 이 현상들은 보통 클라이언트 요청을 처리하는 과정에서 발생한다.
- `Spring Boot`의 경우 내부적으로 여러 `Thread`를 사용하여 작업하는데, `Stack Overflow`는 `Thread` 별로 할당된 `Stack` 메모리 영역에서 발생한다. 보통 재귀 함수 호출이 너무 깊게 들어가거나, `Stack` 프레임의 크기가 너무 커져서 발생한다.
- `Main Thread(e.g. server.js, app.js)`에서 발생하는 경우에는 애플리케이션 전체가 시작되지 않거나 중단될 수 있다.
- 하나의 요청을 처리하는 `Thread`에서는 해당 `Thread`만 종료되고, 다른 `Thread`는 영향을 받지 않는다.
## How can we prevent?
- 재귀 함수의 깊이를 제한하거나, 재귀 대신 반복문을 사용하는 방법을 고민.
- 예외 처리를 통해 적절한 에러를 발생시키도록 한다. 단, `Main Thread`에서는 안하는게 좋은데, 이는 예외 처리할 문제가 아니라 파고들어서 해결해야하는 문제이기 때문에, 직접 문제를 파악하는 것이 좋다. 비즈니스 로직에 대해서는 적절한 에러를 발생시키도록 하고, 로그를 남기는 것이 좋을 것이다.

# How to reverse a string using Stack?
1. 빈 `Stack`을 생성한다.
2. 문자열을 `Stack`에 전부 `push`한다.
3. `Stack`을 전부 `pop`한다.

# Is it possible to implement a special Stack that has a function that returns the minimum value with a time complexity of O(1)?
1. `push/pop`마다 최소값을 비교해서 `min` 변수에 업데이트하는 방법
	- `pop`할 때, `min`  변수가 변경될 가능서이 있다는 것을 염두.
	- 따라서, `pop`할 때는 반드시 전체 요소를 순회해서, `min`을 정해줘야한다. ==> O(n)
2. `Stack`의 각 요소마다 그 값과 그 때의 최소값을 함께 저장하는 방법
	- 이전 요소의 `min`과 현재 값을 비교하여 새로운 `min`을 현재 값과 함께 저장한다.
	- 그러면 현재 `index`에서의 최소값을 모든 데이터들이 지니고 있게 되므로, `push/pop`의 상황에서 문제없이 작동한다.
	- 단, 매번 `min`을 생성해서 저장하므로 메모리 낭비가 커질 수 있다.
3. 2번 방법에서 공간 복잡도를 개선하는 방법
	- `min` 값을 담는 `Stack`을 따로 두고 사용한다.
	- 현재 값과 현재 `min`을 같이 넣어놨던 2번 방법과 달리, 현재 `min`을 같은 `index`의 다른 `Stack`에 만들어가는 것이다.

# What should I use for implementation? Array or Linked List?
1. `Array`로 구현하는 방법
	- JS 내장 배열과 그 메서드(e.g push, pop)를 사용하면 별도의 작업이 필요하지 않다.
1. `Linked List`로 구현하는 방법
	- [[Singly Linked List]]에서 구현한 `push`, `pop` 메서드를 사용하면 안된다!
	- `Stack`은 시간 복잡도가 `O(1)`이어야하는데, [[Singly Linked List]]에서 구현한 것은 `O(n)`이기 때문이다.
	- 따라서, 약간의 수정이 필요하다.

# Implement
## Using Array
- JS의 경우 동적으로 배열 길이가 늘어나기 때문에 상관 없지만, `Stack Overflow` 등을 구현하기 위해 최대 길이를 정해놓고 진행한다.

```js
class Stack{
	constructor(){
		this.stack = [];
		this.maxLength = 10;
	}

	isFull(){
		if(this.stack.length === this.maxLength){
			return true;
		}

		return false;
	}

	isEmpty(){
		if(this.stack.length === 0){
			return true;
		}

		return false;
	}

	push(val){
		if(this.isFull()){
			throw new Error("Stack Overflow");
		}

		this.stack.push(val);
	}

	pop(){
		if(this.isEmpty()){
			throw new Error("Stack Underflow");
		}

		return this.stack.pop();
	}

	peak(){
		if(this.isEmpty()){
			return null;
		}

		return this.stack[this.stack.length - 1];
	}
}
```
## Using [[Singly Linked List]]
- LIFO의 시간 복잡도를 가지게 하기 위해, `head`가 `Stack`의 가장 위, `tail`이 `Stack`의 가장 밑을 의미하도록 한다.
- 여기서의 `push`, `pop` 메서드는 [[Singly Linked List]]의 `unshift`, `shift`와 동일한 기능을 하게 된다.

```js
class Node{
	constructor(val){
		this.val = val;
		this.next = null;
	}
}

class Stack{
	constructor(){
		this.top = null;
		this.bottom = null
		this.length = 0;
		this.maxLength = 10;
	}

	isFull(){
		if(this.length === this.maxLength){
			return true;
		}

		return false;
	}

	isEmpty(){
		if(this.length === 0){
			return true;
		}

		return false;
	}

	push(val){
		if(this.isFull()){
			throw new Error("Stack Overflow");
		}

		let newNode = new Node(val);

		if(!this.top){
			this.top = newNode;
			this.bottom = newNode;
		} else {
			let temp = this.top;
			this.top = newNode;
			this.top.next = temp;
		}

		return ++this.length;
	}

	pop(){
		if(this.isEmpty()){
			throw new Error("Stack Underflow");
		}

		let poped = this.top;

		if(this.length === 1){
			this.top = null;
			this.bottom = null;
		} else {
			this.top = poped.next;
		}

		poped.next = null;
		this.length--;

		return poped;
	}

	peak(){
		return this.top;
	}
}
```
