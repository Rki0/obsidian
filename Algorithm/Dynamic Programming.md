<img width="100%" alt="image" src="https://miro.medium.com/v2/resize:fit:1400/format:webp/1*svQ784qk1hvBE3iz7VGGgQ.jpeg">
image source : https://medium.com/launch-school/recursive-fibonnaci-method-explained-d82215c5498e

- A method for solving a complex problem by breaking it down into a collection of simpler subproblems, solving each of those subproblems just once, and storing their solutions
- 위 이미지는 피보나치 수열에 6이라는 값이 들어갔을 때의 상황을 `Tree`로 구현한 것이다. 불필요한 반복 연산이 많을 뿐더러, 작은 수를 넣었음에도 필요한 연산이 기하급수적으로 증가하는 것을 확인할 수 있다.

# Feature
- 많은 문제에 적용할 수 있는 개념은 아니지만, 이 개념을 적용할 수 있는 문제에 대해서는 그 효율이 기하급수적으로 증가한다.
- `Dynamic Programming`에서 `Dynamic`이란 시간에 따라 변화하는 문제를 표현하기 위한 단어로 쓰인 듯하다.

# Overlapping Subproblems
- A problem is said to have overlapping subproblems, if it can be broken down into subproblems which are reused several times.
- 몇 번이고 다시 사용될 수 있는 하위 문제로 쪼개질 수 있는 것을 `Overlapping Problems`라고 한다.

# Optimal Substructure
- A problem is said to have optimal substructure, if an optimal solution can be constructed from optimal solutions of its subproblems.
- 어떤 문제의 최적의 답이 그 하위 문제의 최적의 답을 통해 만들어질 수 있다면, 그 문제는 `Optimal Substructure`를 가지고 있다고 할 수 있다.
- 가령, 최단 경로를 구할 때, `A -> D`가 최단 경로라면, 그 하위 경로인 `A -> C`의 최단 경로는 `A -> D`에서의 `A -> C`와 동일할 것이고, 하위 경로인 `A -> B`의 최단 경로 또한 `A -> D`에서의 `A -> B`와 동일할 것이다.

# Using past knowledge!
- `DP`를 할 때는 값을 잘 기억해놓고, 동일한 연산이 나올 때 그 값을 다시 꺼내서 사용하자!
- 값을 기억하는 방법은 크게 두 가지로 나눌 수 있다.
## Memoization
- Storing the results of expensive function calls and returning the cached result when the same inputs occur again.
- Top-Down
## Tabulation
- Storing the result of a previous result in a "table"(usually an array)
- Usually done using iteration
- Better space complexity can be achieved using tabulation
- Bottom-Up

# Example
- 피보나치 수열 문제가 대표적인 `DP` 문제이다.
- 일반적인 방법으로 풀면 시간 복잡도가 `O(2^n)`으로 최악의 성능을 보인다.

```js
function fibonacci(n){
	if(n <= 2){
		return 1;
	}

	return fibonacci(n - 1) + fibonacci(n - 2);
}
```
## Memoization Version
- `memo`에 이미 연산된 `n`에 대한 정보를 저장한다.
- 다른 재귀에서 사용될 때, 굳이 모든 연산을 다시 하지않고 저장된 값을 꺼내 쓰는 것으로 해결할 수 있다!
- 따라서, 시간 복잡도가 `O(n)`으로 줄어든다.

```js
function fibonacci(n, memo = []){
	if(memo[n] !== undefined){
		return memo[n];
	}

	if(n <= 2){
		return 1;
	}

	let res = fibonacci(n - 1, memo) + fibonacci(n - 2, memo);

	memo[n] = res;

	return res;
}
```

- 혹은 `base case`를 미리 넣어놓는 방법으로 사용할 수 도 있다.
```js
function fibonacci(n, memo = [undefined, 1, 1]){
	if(memo[n] !== undefined){
		return memo[n];
	}

	let res = fibonacci(n - 1, memo) + fibonacci(n - 2, memo);

	memo[n] = res;

	return res;
}
```
## Tabulation Version
- `Memoization`의 경우 `n`이 커짐에 따라 콜 스택 오버플로우가 발생할 수도 있는데, `Tabulation` 방법은 그런 걱정이 없다.
- 재귀가 아니라 반복문이고, 공간 복잡도가 훨씬 작기 때문이다.

```js
function fibonacci(n){
	if(n <= 2){
		return 1;
	}

	let fibNums = [0, 1, 1];

	for(let i = 3; i <= n; i++){
		fibNums[i] = fibNums[i - 1] + fibNums[i - 2];
	}

	return fibNums[n];
}
```