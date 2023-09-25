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
			2. Update the previous object 

# Implement
```js

```