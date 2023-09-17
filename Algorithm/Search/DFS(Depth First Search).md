<img width="100%" alt="image" src="https://www.guru99.com/images/1/020820_0543_BreadthFirs1.png">
image source : https://www.guru99.com/images/1/020820_0543_BreadthFirs1.png
# Feature
- 넓은 `Tree`에 대해서 좋다. `BFS`에 비해 공간 복잡도가 작기 때문이다. 하나를 깊게 내려가기 때문에 옆으로 넓은 것들에 대해 더 강하다!
- 깊은 `Tree`에 대해서는 좋지 않다. 깊은 `Tree`의 하나의 경로를 끝까지 들어가야하기 때문이다. 공간 복잡도가 높아진다.
- 시간 복잡도는 `BFS`와 다를게 없다. 모든 노드를 한번씩 순회하기 때문이다.

# Pseudo Code
- 본인을 우선 탐색하는지, 중간에 탐색하는지, 나중에 탐색하는지에 따라 코드가 약간
## Pre-Order
1. Create a variable to store the values of the nodes visited
2. Store the root of the BST in a variable called current
3. Write a helper function which accepts a node
	1. Push the value of the node to the variable that stores the values
	2. If the node has a left property, call the helper function with the left property on the node
	3. If the node has a right property, call the helper function with the right property on the node
4. Invoke the helper function with the current variable
5. Return the array of values
## Post-Order
1. Create a variable to store the values of the nodes visited
2. Store the root of the BST in a variable called current
3. Write a helper function which accepts a node
	1. If the node has a left property, call the helper function with the left property on the node
	2. If the node has a right property, call the helper function with the right property on the node
	3. Push the value of the node to the variable that stores the values
4. Invoke the helper function with the current variable
5. Return the array of values
## In-Order
1. Create a variable to store the values of the nodes visited
2. Store the root of the BST in a variable called current
3. Write a helper function which accepts a node
	1. If the node has a left property, call the helper function with the left property on the node
	2. Push the value of the node to the variable that stores the values
	3. If the node has a right property, call the helper function with the right property on the node
4. Invoke the helper function with the current variable
5. Return the array of values

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

	DFSPreOrder(){
		let data = [];

		function traverse(node){
			data.push(node.val);

			if(node.left){
				traverse(node.left);
			}

			if(node.right){
				traverse(node.right);
			}
		}

		traverse(this.root);

		return data;
	}

	DFSPostOrder(){
		let data = [];

		function traverse(node){
			if(node.left){
				traverse(node.left);
			}

			if(node.right){
				traverse(node.right);
			}

			data.push(node.val);
		}

		traverse(this.root);

		return data;
	}

	DFSInOrder(){
		let data = [];

		function traverse(node){
			if(node.left){
				traverse(node.left);
			}

			data.push(node.val);

			if(node.right){
				traverse(node.right);
			}
		}

		traverse(this.root);

		return data;
	}
}
```