
# Feature
- 이진 트리의 한 유형으로, 왼쪽 하위 트리의 모든 노드가 부모 노드보다 작고, 오른쪽 하위 트리의 모든 노드가 부모 노드보다 크다.
- 최대 두 개의 자식 노드를 가지며, 왼쪽 자식과 오른쪽 자식으로 구분한다.
- data set을 이분할 때, 무엇을 기준으로 분할할지에 따라 특화된 이진 트리를 만들 수 있다.

# Time Complexity

# Pseudo Code
## insertion
1. Create a new Node
2. Starting at the root
	1. Check if there is a root, if not - the root now becomes that new node!
	2. If there is a root, check if the value of the new node is greater than or less than the value of the root
	3. If it is greater
		1. Check to see if there is a node to the right
			1. If there is, move to that node and repeat these steps
			2. If there is not, add that node as the right property
	4. If it is less
		1. Check to see if there is a node to the left
			1. If there is, move to that node and repeat these steps
			2. If there is not, add that node as the left property
## find
1. Starting at the root
2. Check if there is a root, if not - we're done searching!
3. If there is a root, check if the value of the new node is the value we are looking for. If we found it, we're done!
4. If not, check to see if the value is greater than or less than the value of the root
5. If it is greater
	1. Check to see if there is a node to the right
	2. If there is, move to that node and repeat these steps
	3. If there is not, we're done searching!
6. If it is less
	1. Check to see if there is node to the left
	2. If there is, move to that node and repeat these steps
	3. If there is not, we're done searching!
## BFS
1. Create a queue(this can be an array) and a variable to store the values of nodes visited
2. Place the root node in the queue
3. Loop as long as there is anything in the queue
	1. Dequeue a node from the queue and push the value of the node into the variable that stores the nodes
	2. If there is a left property on the node dequeued - add it to the queue
	3. If there is a right property on the node dequeued - add it to the queue
4. Return the variable that stores the values
## DFS(Pre-Order)
1. Create a variable to store the values of the nodes visited
2. Store the root of the BST in a variable called current
3. Write a helper function which accepts a node
	1. Push the value of the node to the variable that stores the values
	2. If the node has a left property, call the helper function with the left property on the node
	3. If the node has a right property, call the helper function with the right property on the node
4. Invoke the helper function with the current variable
5. Return the array of values

# Implement

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

	insert(val){
		const newNode = new Node(val);

		if(!this.root){
			this.root = newNode;
			return this;
		}

		let current = this.root;

		while(current){
			if(val === current.val){
				return undefined;
			}

			if(val < current.val){
				if(!current.left){
					current.left = newNode;
					return this;
				}

				current = current.left;
				continue;
			}

			if(val > current.val){
				if(!current.right){
					current.right = newNode;
					return this;
				}

				current = current.right;
				continue;
			}
		}

		return this;
	}

	find(val){
		if(!this.root){
			return undefined;
		}

		let current = this.root;

		while(current){
			if(val < current.val){
				current = current.left;
				continue;
			}

			if(val > current.val){
				current = current.right;
				continue;
			}

			return current;
		}

		return undefined;
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

## insertion - Recursive Version

```js
class BinarySearchTree{
	constructor(){
		this.root = null;
	}

	insert(val){
		const newNode = new Node(val);

		if(!this.root){
			this.root = newNode;
			return this;
		}

		return this.insertNode(this.root, newNode);
	}

	insertNode(node, newNode){
		if(newNode.val === node.val){
			return undefined;
		}

		if(newNode.val < node.val){
			if(!node.left){
				node.left = newNode;
				return this;
			}

			return this.insertNode(node.left, newNode);
		}

		if(newNode.val > node.val){
			if(!node.right){
				node.right = newNode;
				return this;
			}

			return this.insertNode(node.right, newNode);
		}
	}
}
```

## find - Recursive Version

```js
class BinarySearchTree{
	constructor(){
		this.root = null;
	}

	find(val){
		if(!this.root){
			return undefined;
		}

		return this.findNode(val, this.root);
	}

	findNode(val, node){
		if(!node){
			return undefined;
		}

		if(val < node.val){
			return this.findNode(val, node.left);
		}

		if(val > node.val){
			return this.findNode(val, node.right);
		}

		return node;
	}
}
```