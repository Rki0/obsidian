<img width="100%" alt="image" src="https://www.guru99.com/images/1/020820_0543_BreadthFirs1.png">
image source : https://www.guru99.com/images/1/020820_0543_BreadthFirs1.png
# Feature
- 넓은 `Tree`에 대해서는 좋지 않다. `Queue`에 점점 더 많은 `node`를 저장해나가야하기 때문에 공간 복잡도가 나쁘기 때문이다.
- 깊은 `Tree`에 대해서 좋다. `Queue`에 많은 `node`가 들어가지 않기 때문에 공간 복잡도가 좋기 때문이다.
- 시간 복잡도는 `DFS`와 다를게 없다. 모든 노드를 한 번씩 순회하기 때문이다.

# Pseudo Code
1. Create a queue(this can be an array) and a variable to store the values of nodes visited
2. Place the root node in the queue
3. Loop as long as there is anything in the queue
	1. Dequeue a node from the queue and push the value of the node into the variable that stores the nodes
	2. If there is a left property on the node dequeued - add it to the queue
	3. If there is a right property on the node dequeued - add it to the queue
4. Return the variable that stores the values

# Implement
- [[Binary Search Tree(BST)]]에 대해서 구현한 것이지만, 기본적인 골조는 같으므로 다양한 자료 구조에서 변형해서 사용하면 된다.
- 아래 코드는 같은 레벨의 노드를 판단하지 않기 때문에, 순회에만 쓰일 수 있는 방법이다.
- 또한, `left`, `right`, 즉, `BST`를 가정한 `BFS`이므로 다른 타입의 `Tree`에서는 사용하기 힘들 수 있다.

```js
class Node{
	constructor(val){
		this.val = val;
		this.left = null;
		this.right = null;
	}
}

class BinarySearchTree{
	constructor(){
		this.root = null;
	}

	BFS(){
		let data = [];
		let queue = [];

		let node = this.root;

		queue.push(node);

		while(queue.length > 0){
			node = queue.shift();

			data.push(node.val);

			if(node.left){
				queue.push(node.left);
			}

			if(node.right){
				queue.push(node.right);
			}
		}

		return data;
	}
}
```

## BFS - How to determine nodes at the same level?
- 코딩 테스트에서는 `BFS`를 활용하여, 같은 레벨에 있는 노드를 활용해 어떤 기능을 구현하는 문제가 종종 있다.
- 예를들어, [[Trie]]나 `Graph`가 있을 수 있겠다. 즉, 자식의 개수가 일정하지 않은 형태이다.
- 따라서, 여러 자식을 가질 때 같은 레벨의 노드인지를 판단할 수 있는 방법을 추가하였다.

```js
class Node{
	constructor(){
		this.children = {};
		this.endOfWord = false;
	}
}

class BinarySearchTree{
	constructor(){
		this.root = new Node();
	}

	BFS(){
		let queue = [];

		let node = this.root;

		queue.push(node);

		while(queue.length > 0){
            const sameLevelLength = queue.length;

            for(let i = 0; i < sameLevelLength; i++){
                node = queue.shift();
            
                for(let child in node.children){
                    queue.push(node.children[child]);
                }
            }
		}
	}
}
```