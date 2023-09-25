<img width="100%" alt="image" src="https://media.geeksforgeeks.org/wp-content/uploads/20230224120836/dijkstras6-(1).png">
image source : https://www.geeksforgeeks.org/introduction-to-dijkstras-shortest-path-algorithm/
# Feature
- 다익스트라 최단 경로 알고리즘으로 보통 다익스트라 알고리즘으로 불린다.
- 가중치가 양수인 경우 사용하는 알고리즘이다.
- [[Priority Queue]]를 사용해서 다음에 방문할 노드를 결정한다.

# Flow
1. Every time we look to visit a new node, we pick the node with the smallest known distance to visit first
2. Once we've moved to the node we're going to visit, we look at each of its neighbors
3. For each neighbors node, we calculate the distance by summing the total edges that lead to the node we're checking from the starting node
4. If the new total distance to a node is less than the previous total, we store the new shorter distance for that node

# Pseudo Code
1. This function should accept a starting and ending vertex
2. Create an object(we'll call it distances) and set each key to be every vertex in the adjacency list with a value of infinity, except for the starting vertex which should have a value of 0
3. After setting a value in the distances object, add each vertex with a priority of Infinity to the priority queue, except the starting vertex, which should have a priority of 0 because that's where we begin
4. Create another object called previous and set each key to be every vertex in the adjacency list with a value of null
5. Start looping as long as there is anything in the priority queue
	1. Dequeue a vertex from the priority queue
	2. If that vertex is the same as the ending vertex - we are done!
	3. Otherwise loop through each value in the adjacency list at that vertex
		1. Calculate the distance to that vertex from the starting vertex
		2. If the distance is less that what is currently stored in our distances object
			1. Update the distances object with new lower distance
			2. Update the previous object to contain that vertex
			3. Enqueue the vertex with the total distance from the start node

# Implement
```js
class Node{
	constructor(val, priority){
		this.val = val;
		this.priority = priority;
	}
}

class PriorityQueue{
	constructor(){
		this.values = [];
	}

	enqueue(val, priority){
		let newNode = new Node(val, priority);

		this.values.push(newNode);
		
		this.bubbleUp();
	}

	bubbleUp(){
		let index = this.values.length - 1;
		const element = this.values[index];

		while(index > 0){
			let parentIndex = Math.floor((index - 1) / 2);
			let parent = this.values[parentIndex];

			if(element.priority >= parent.priority){
				break;
			}

			this.values[parentIndex] = element;
			this.values[index] = parent;

			index = parentIndex;
		}
	}

	dequeue(){
		const min = this.values[0];
		const end = this.values.pop();

		if(this.values.length > 0){
			this.values[0] = end;

			this.sinkDown();
		}

		return min;
	}

	sinkDown(){
		let index = 0;
		const element = this.values[0];

		while(true){
			let leftChildIndex = 2 * index + 1;
			let rightChildIndex = 2 * index + 2;

			let leftChild = null;
			let rightChild = null;
			let swap = null;

			if(leftChildIndex < this.values.length){
				leftChild = this.values[leftChildIndex];

				if(leftChild.priority < element.priority){
					swap = leftChildIndex;
				}
			}

			if(rightChildIndex < this.values.length){
				rightChild = this.values[rightChildIndex];

				if(
					(swap === null && rightChild.priority > element.priority) ||
					(swap !== null && rightChild.priority > leftChild.priority)
				){
					swap = rightChildIndex;
				}
			}

			if(swap === null){
				break;
			}

			this.values[index] = this.values[swap];
			this.values[swap] = element;

			index = swap;
		}
	}
}

class WeightedGraph{
	constructor(){
		this.adjacencyList = {};
	}

	addVertex(vertex){
		if(!this.adjacencyList[vertex]){
			this.adjacencyList[vertex] = [];
		}
	}

	addEdge(vertex1, vertex2, weight){
		this.adjacencyList[vertex1].push({node : vertex2, weight});
		this.adjacencyList[vertex2].push({node : vertex1, weight});
	}

	Dijkstra(start, finish){
		const nodes = new PriorityQueue();
		const distances = {};
		const previous = {};
		let path = [];
		let smallest;

		// build up initial state
		for(let vertex in this.adjacencyList){
			if(vertex === start){
				distances[vertex] = 0;
				nodes.enqueue(vertex, 0);
			} else {
				distances[vertex] = Infinity;
				nodes.enqueue(vertex, Infinity);
			}

			previous[vertex] = null;
		}

		// as long as there is something to visit
		while(nodes.values.length){
			smallest = nodes.dequeue().val;

			if(smallest === finish){
				// build up path to return at end
				while(previous[smallest]){
					path.push(smallest);
					smallest = previous[smallest];
				}

				break;
			}

			if(smallest || distances !== Infinity){
				for(let neighbor in this.adjacencyList[smallest]){
					// find neighboring node
					let nextNode = this.adjacencyList[smallest][neighbor];

					// calculate new distance to neighboring node
					let candidate = distances[smallest] + nextNode.weight;

					let nextNeighbor = nextNode.node;

					if(candidate < distances[nextNeighbor]){
						// updating new smallest distance to neighbor
						distances[nextNeighbor] = candidate;

						// updating previous - How we got to neighbor
						previous[nextNeighbor] = smallest;

						// enqueue in priority queue with new priority
						node.enqueue(nextNeighbor, candidate);
					}
				}
			}
		}

		return path.concat(smallest).reverse();
	}
}
```