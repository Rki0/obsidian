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