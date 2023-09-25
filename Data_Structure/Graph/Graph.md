<img width="100%" alt="image" src="https://www.shiksha.com/online-courses/articles/wp-content/uploads/sites/11/2021/09/Graph-Data-Structure.png">
image source : https://www.shiksha.com/online-courses/articles/graphs-in-data-structure-types-representation-operations/
# Feature
- 객체 간의 관계를 정점(Vertex)와 간선(Edge)으로 나타내는 자료구조
- 부모/자식 관계가 존재하지 않고, 인접한 관계가 있을 뿐이다.
- 따라서, 모든 정점이 독립적인 존재이다.

# Terminology
- 정점(Vertex)
- 간선(Edge)
- 인접(Adjacent) : 어떠한 노드에 연결되어 있는 다른 노드를 인접 노드라고 한다.
- 부속(Incident) : A 정점과 B 정점을 연결하는 간선을 정점 A와 B에 부속된 간선이라고 한다.
- 차수(Degree) : 인접한 노드의 개수.
	- 방향 그래프에서는 들어오는 간선의 개수를 "진입 차수", 나가는 간선의 개수를 "진출 차수"라고 한다.
- 경로(Path) : 정점 A(출발지)에서 정점 B(목적지)로 이어지는 일련의 간선들.
	- 경로를 구성하는 간선의 개수를 "경로 길이(Path Length)"라고 한다.
	- "단순 경로(Simple Path)"는 동일한 노드를 중복해서 포함하지 않는 경로를 의미한다.
	- "사이클(Cycle)"은 그래프 내에서 시작 노드와 종료 노드가 동일한 "단순 경로"를 의미한다. 따라서, 사이클은 최소 3개 이상의 노드로 구성된다.
- 루프(Loop) : 본인에서 출발해서 본인으로 바로 돌아오는 순환을 가지는 것을 의미한다. 즉, 본인에게서 나온 간선이 바로 본인으로 다시 돌아오는 구조이다.

# Type of Graph
1. 비가중 & 가중 그래프
	- 비가중 그래프
		- 모든 간선이 동일한 값을 가진 그래프
		- 간선의 가중치에 대한 정보가 없거나, 필요하지 않은 경우 사용한다.
	- 가중 그래프
		- 간선이 가중치(Weight)를 가지는 그래프
		- 간선 사이의 비용, 길이, 거리, 친밀도 등 다양한 것을 표현할 수 있다.
		- 무방향 + 가중 = 무방향 가중 그래프
		- 방향 + 가중 = 방향 가중 그래프(소셜 네트워크 등에서 활용)
		- 노드 사이의 연결 관계를 추가할 수 있어서 응용 분야가 많다.
2. 비순환 & 순환 그래프
	- 비순환 그래프
		- 그래프 내에 어떠한 사이클도 포함되어 있지 않은 그래프
		- 대표적으로 [[Tree]]가 있다.
		- 모든 노드 사이에 정확히 하나의 경로만 존재한다.
	- 순환 그래프
		- 그래프 내에 하나 이상의 사이클이 존재하는 그래프
		- "사이클"은 "단순 경로"로 이루어지므로, 동일한 노드를 중복해서 사용할 수 없다.
3. 기타
	- 연결 & 비연결 그래프
	- 완전 그래프
	- 부분 그래프
	- 다중 그래프
	- etc...

# How can I make computers understand Graph?
## 인접 행렬(Adjacency Matrix)
- 그래프의 정점들 간의 연결 관계를 2차원 배열로 표현하는 방식
- `arr[i][j]`는 정점 `i`와 정점 `j`가 연결되어 있으면 1, 아니면 0으로 표시한다.
- 무방향 그래프일 때는 대각선을 기준으로 대칭 구조이다.
- 구현하기 쉽고, 두 정점 간의 연결 여부를 직관적을 확인 가능
- 단, 정점은 많지만 간선이 적은 그래프의 경우 비효율적이다. 사용되지 않는 간선임에도 불구하고 0을 넣기 위해 공간을 생성해야하는 문제가 발생하기 때문이다.
## 인접 리스트(Adjacency List)
- 각 정점이 인접하고 있는 정점을 [[Linked List]]로 표현하는 방식
- 인접 행렬에서는 간선 저장을 통해 그래프를 표현했다면, 인접 리스트는 각 정점마다 해당 정점과 연결된 정점들의 리스트를 가진다.
- 실제로 연결된 간선만 저장하기 때문에, 메모리 절약이 가능하다.
- 단, 연결이 많아지면 그 효과가 떨어질 수는 있다.
## Matrix or List? Which is better?
- 정답은 없지만, `Adjacency List`로 구현을 하는게 더 좋다고 생각된다.
- 현실 속 대부분의 데이터는 큰 `Graph` 속에서 아주 작은 수의 연결을 가지고 있기 때문이다.
- 연결이 적은 그래프에서 `Matrix`는 공간을 많이 차지하고, `List`는 공간을 덜 차지한다.
- `Matrix`는 모든 간선을 순회하는게 느리고, `List`는 빠르다.
- `Matrix`는 특정 간선을 찾는 것이 빠르고, `List`는 느리다.

# Time Complexity
- `|V|` : number of vertices
- `|E|` = number of edges
## Adjacency Matrix
- Add Vertex : O(`|V^2|`) - 2차원 배열이기 때문에 하나의 `vertex`가 추가되면 `row`, `col`을 전부 추가해야하기 때문
- Add Edge : O(1)
- Remove Vertex : O(`|V^2|`)
- Remove Edge : O(1)
- Query = O(1)
- Storage = O(`|V^2|`)
## Adjacency List
- Add Vertex : O(1)
- Add Edge : O(1)
- Remove Vertex : O(`|V| + |E|`)
- Remove Edge : O(`|E|`)
- Query = O(`|V| + |E|`)
- Storage = O(`|V| + |E|`)

# DFS & BFS of Graph
## DFS
- [[Tree]]에서와 다른 점은 `Graph`에서 사용하는 `DFS`는 **방문했던 정점을 저장하는 방법**이 추가되어야 무한 루프가 발생하지 않고 진행할 수 있다는 것
- 방문했던 정점을 전역 변수로 관리하거나 특정 노드의 방문 여부를 저장하는 방법으로 이를 구현할 수 있다.
## BFS
- `Graph`는 부모/자식 관계가 없기 때문에 다른 레벨을 판단하기가 어려운데, 이 때 **등고선**을 보는 것처럼 그려보면 이해가 편하다!
- [[Dijkstra Algorithm]] : 가중치가 양수인 경우 사용하는 알고리즘
- 벨만포드 알고리즘 : 가중치가 음수인 경우 사용하는 알고리즘

# Pseudo Code
## addVertex
1. Write a method called addVertex, which accepts a name of a vertex
2. It should add a key to the adjacency list with the name of the vertex and set its value to be an empty array
## addEdge
1. This function should accept two vertices, we can call them vertex1 and vertex2
2. The function should find in the adjacency list the key of vertex1 and push vertex2 to the array
3. The function should find in the adjacency list the key of vertex2 and push vertex1 to the array
## removeEdge
1. This function should accept two vertices, we can call them vertex1 and vertex2
2. The function should reassign the key of vertex1 to be an array that does not contain vertex2
3. The function should reassign the key of vertex2 to be an array that does not contain vertex1
## removeVertex
1. The function should accept a vertex to remove
2. The function should loop as long as there are any other vertices in the adjacency list for that vertex
3. Inside of the loop, call our `removeEdge()` function with the vertex we are removing and any values in the adjacency list for that vertex
4. Delete the key in the adjacency list for that vertex
## DFS - Recursive Version
1. The function should accept a staring node
2. Create a list to store the end result, to be returned at the very end
3. Create an object to store visited vertices
4. Create a helper function which accept a vertex
	1. The helper function should return early if the vertex is empty
	2. The helper function should place the vertex it accepts into the visited object and push that vertex into the result array
	3. Loop over all of the values in the adjacency list for that vertex
	4. If any of those values have not been visited, recursively invoke the helper function with that vertex
5. Invoke the helper function with the starting vertex
6. Return the result array
## DFS - Iterative Version
1. The function should accept a staring node
2. Create a stack to help use keep track of vertices(use a list/array)
3. Create a list to store the end result, to be returned at the very end
4. Create an object to store visited vertices
5. Add the starting vertex to the stack, and mark it visited
6. While the stack has something in it:
	1. Pop the next vertex from the stack
	2. If the vertex hasn't been visited yet:
		1. Mark it as visited
		2. Add it to the result list
		3. Push all of its neighbors into the stack
7. Return the result array
## BFS
1. This function should accept a starting vertex
2. Create a queue(you can use an array) and place the starting vertex in it
3. Create an array to store the nodes visited
4. Create an object to store nodes visited
5. Mark the starting vertex as visited
6. Loop as long as there is anything in the queue
7. Remove the first vertex from the queue and push it into the array that stores nodes visited
8. Loop over each vertex in the adjacency list for the vertex you are visiting
9. If it is not inside the object that stores nodes visited, mark it as visited and enqueue that vertex
10. Once you have finished looping, return the array of visited nodes

# Implement
- 해당 `Graph`는 무방향 그래프이다.
- 에러 핸들링, 예외 처리는 하지 않았으므로 알맞게 추가해서 사용하도록 하자.
- `this.adjacencyList`는 하나의 정점이 `key`이며, `value`는 그 정점과 연결되어 있는 정점들을 모아놓은 배열이다.

```js
class Graph{
	constructor(){
		this.adjacencyList = {};
	}

	addVertex(vertex){
		if(!this.adjacencyList[vertex]){
			this.adjacencyList[vertex] = [];
		}
	}

	addEdge(vertex1, vertex2){
		this.adjacencyList[vertex1].push(vertex2);
		this.adjacencyList[vertex2].push(vertex1);
	}

	removeEdge(vertex1, vertex2){
		this.adjacencyList[vertex1] = this.adjacencyList[vertex1].filter(
			(vertex) => vertex !== vertex2
		);

		this.adjacencyList[vertex2] = this.adjacencyList[vertex2].filter(
			(vertex) => vertex !== vertex1
		);
	}

	removeVertex(vertex){
		while(this.adjacencyList[vertex].length){
			const adjacentVertex = this.adjacencyList[vertex].pop();

			this.removeEdge(vertex, adjacentVertex);
		}

		delete this.adjacencyList[vertex];
	}

	DFS(start){
		const result = [];
		const visited = {};

		const helper = (vertex) => {
			if(!vertex){
				return null;
			}

			visited[vertex] = true;
			result.push(vertex);

			this.adjacencyList[vertex].forEach((neighbor) => {
				if(!visited[neighbor]){
					return helper(neighbor);
				}
			});
		}

		helper(start);

		return result;
	}

	BFS(start){
		const queue = [start];
		const result = [];
		const visited = {};
		visited[start] = true;

		let currentVertex;

		while(queue.length){
			currentVertex = queue.shift();
			result.push(currentVertex);

			this.adjacencyList[currentVertex].forEach((neighbor) => {
				if(!visited[neighbor]){
					visited[neighbor] = true;
					queue.push(neighbor);
				}
			});
		}

		return result;
	}	
}
```

## DFS - Iterative Version
- 정점을 처리하는 순서로 인해서 `Recursive Version`과는 표시되는 결과가 다르다.
- 물론, 어떤 방법을 쓰더라도 `DFS`라는 점은 똑같다.

```js
class Graph{
	constructor(){
		this.adjacencyList = {};
	}

	DFS(start){
		const stack = [start];
		const result = [];
		const visited = {};
		let currentVertex;

		visited[start] = true;

		while(stack.length){
			currentVertex = stack.pop();
			result.push(currentVertex);

			this.adjacencyList[currentVertex].forEach((neighbor) => {
				if(!visited[neighbor]){
					visited[neighbor] = true;
					stack.push(neighbor);
				}
			});
		}

		return result;
	}
}
```