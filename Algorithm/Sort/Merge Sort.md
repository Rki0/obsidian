<img width="100%" alt="image" src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/e6/Merge_sort_algorithm_diagram.svg/600px-Merge_sort_algorithm_diagram.svg.png">
image source : https://upload.wikimedia.org/wikipedia/commons/thumb/e/e6/Merge_sort_algorithm_diagram.svg/600px-Merge_sort_algorithm_diagram.svg.png
# Principle
- 배열을 0개 혹은 1개의 원소를 가질 때까지 최대한 잘게 쪼갠다.
- 0개 혹은 1개의 원소를 가진 배열은 항상 정렬되어 있다는 사실을 이용한다.
- 쪼개진 배열을 재조합하며 정렬된 상태를 만들어간다.

# Feature
- `Divide and Conquer`과 `Recursion`을 사용하는 정렬 방법이다.
- 더 작은 배열로 분할한 뒤, 각 부분 배열을 정렬하고 병합하는 과정을 재귀적으로 반복한다.
- 즉, `merging`과 `sorting`의 콤비네이션이라고 생각하면 된다.

# Time Complexity
- General, Worst, Best Case : O(n log n) - 정렬(comparisons per decomposition)이 O(n), 분할(decompositions)이 O(log n)이기 때문.

# Space Complexity
- O(n)

# Pseudo Code
In order to implement `Merge Sort`, it's useful to first implement a function responsible for merging two sorted arrays.  
Given two arrays which are sorted, this helper function should create a new array which is also sorted, and consists of all of the elements in the two input arrays.  
This function should run in `O(n+m)` time and `O(n+m)` space and should not modify the parameters passed to it.

- merge
	1. Create an empty array, take a look at the smallest values in each input array
	2. While there are still values we haven't looked at..
		1. If the value in the first array is smaller than the value in the second array, push the value in the first array into our results and move on to the next value in the first array.
		2. If the value in the first array is larger than the value in the second array, push the value in the second array into our results and move on to the next value in the second array.
		3. Once we exhaust one array, push in all remaining values from the other array.
- merge sort
	1. Break up the array into halves until you have arrays that are empty or have one element.
	2. Once you have smaller sorted array, merge those arrays with other sorted arrays until you are back at the full length of the array.
	3. Once the array has been merged back together, return the merged(and sorted) array.

# Implement
- `merge`와 `mergeSort`를 구분해서 구현한다.
- `merge`는 정렬과 병합을 담당하고, `mergeSort`는 `merge`의 재귀적 사용을 담당한다.
## merge
- `sorting`과 `merging`를 한번에 담당하고 있다.
- 병합하고자하는 두 개의 배열을 순회하면서 크기 비교를 하고 새롭게 정렬된 배열을 생성해 나간다.
- 하나의 배열이 먼저 순회가 끝나면 다른 배열의 나머지 부분을 뒤에 그대로 이어준다.
- 가장 작은 단위에서부터 시작해서 `sorting`되며 합쳐져왔기 때문에 정렬된 상태를 유지하므로, 그대로 이어줄 수있다.

```js
function merge(arr1, arr2){
	let result = [];

	let p1 = 0;
	let p2 = 0;

	while(p1 < arr1.length && p2 < arr2.length){
		if(arr[p1] < arr[p2]){
			result.push(arr1[p1]);
			p1++;
		} else {
			result.push(arr2[p2]);
			p2++;
		}
	}

	while(p1 < arr1.length){
		result.push(arr[p1]);
		p1++;
	}

	while(p2 < arr2.length){
		result.push(arr[p2]);
		p2++;
	}

	return result;
}
```

## mergeSort
- 배열을 절반씩 잘라서 사용하기 위한 작업을 진행한다.
- 분리된 두 배열에 대해 `merge`를 호출하여 계속해서 정렬 및 병합된 상태로 만들어준다.
- 이를 `mergeSort`를 통해 다시 분리하고, 또 다시 `merge`로 정렬 및 병합을 하며 나아간다.

```js
function mergeSort(arr){
	if(arr.length === 1){
		return arr;
	}

	const mid = Math.floor(arr.length / 2);

	let left = mergeSort(arr.slice(0, mid));
	let right = mergeSort(arr.slice(mid));

	return merge(left, right);
}
```