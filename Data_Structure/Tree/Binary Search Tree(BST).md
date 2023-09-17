
# Feature
- 이진 트리의 한 유형으로, 왼쪽 하위 트리의 모든 노드가 부모 노드보다 작고, 오른쪽 하위 트리의 모든 노드가 부모 노드보다 크다.
- 최대 두 개의 자식 노드를 가지며, 왼쪽 자식과 오른쪽 자식으로 구분한다.
- data set을 이분할 때, 무엇을 기준으로 분할할지에 따라 특화된 이진 트리를 만들 수 있다.

# Time Complexity

# Pseudo Code
## Insertion
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
	}
}
```

## Insertion - Recursively

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

		this.insertNode(this.root, newNode);
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

			this.insertNode(node.left, newNode);
		}

		if(newNode.val > node.val){
			if(!node.right){
				node.right = newNode;
				return this;
			}

			this.insertNode(node.right, newNode);
		}
	}
}
```