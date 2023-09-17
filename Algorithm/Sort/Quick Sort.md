<img width="100%" alt="image" src="https://wat-images.s3.ap-south-1.amazonaws.com/images/course/ci6ldqnqthum/Quick_Sort_0.png">
image source : https://wat-images.s3.ap-south-1.amazonaws.com/images/course/ci6ldqnqthum/Quick_Sort_0.png
# Principle
- 0개 혹은 1개의 원소를 가진 배열은 항상 정렬되어 있다는 사실을 이용한다.
- `pivot`으로 지정한 하나의 원소를 가지고, 정렬된 배열에서 `pivot`이 끝부분이어야하는 인덱스를 찾는다.

# Feature
- Worst Case가 되는 것을 최대한 억제하기 위해, 중앙값을 `pivot`으로 삼는 것으로 진행하는 방법도 있다.

# Time Complexity
- General, Best Case : O(n log n) - 정렬(comparisons per decomposition)이 O(n), 분할(decompositions)이 O(log n)이기 때문.
- Worst Case : O(n^2) - 정렬된 데이터에 대해서 `Quick Sort`를 진행할 경우, 정렬과 분할이 전부 O(n)이기 때문.

# Pseudo Code
- In order to implement `Quick Sort`, it's useful to first implement a function responsible arranging elements in an array on either side of a pivot.
- Given an array, this helper functions should designate an element as the pivot.
- It should then rearrange elements in the array so that all values less than the pivot are moved to the left of the pivot, and all values greater than the pivot are moved to the right of the pivot.
- The order of the elements on either side of the pivot doesn't matter!
- The helper should do the in place, that is, it should not create a new array.
- When complete, the helper should return the index of the pivot.
- The runtime of `Quick Sort` depends in parts on how one selects the pivot.
- Ideally, the pivot should be chosen so that it's roughly the median value in the data set you're sorting.
## pivot
1. It will help to accept three arguments : an array, a start index, and an end index(these can default to 0 and the array length minus 1, respectively)
2. Grab the pivot from the start of the array(this can be changed if you want, like middle or end of the array)
3. Store the current pivot index in a variable(this will keep track of where the pivot should end up)
4. Loop through the array from the start until the end
	1. If the pivot is greater than the current element, increment the pivot index variable and then swap the current element with the element at the pivot index.
5. Swap the starting element(i.e. the pivot) with the pivot index.
6. Return the pivot index
## quick sort
1. Call the pivot helper on the array
2. When the helper returns to you the updated pivot index, recursively call the pivot helper on the subarray to the left of that index, and the subarray to the right of that index.
3. Your base case occurs when you consider a subarray with less than 2 elements.

# Implement

## pivot
- 가장 앞의 인덱스를 `pivot`으로 사용한다.
- 두 번째 인덱스부터 순회하며 `swapIdx`를 증가시키는 것으로 `pivot`이 정렬 상태에서 들어가야할 위치를 탐색한다.
- 순회를 마치고, 최종적으로 얻어진 `swapIdx`와 `pivot`을 스왑한다. `pivot`을 첫 인덱스로 설정했었기에 `start`와 변경하는 것으로 이를 구현할 수 있다.

```js
function swap(arr, idx1, idx2){
	[arr[idx1], arr[idx2]] = [arr[idx2], arr[idx1]];
}

function pivot(arr, start = 0, end = arr.length - 1){
	let pivot = arr[start];
	let swapIdx = start;

	for(let i = start + 1; i < arr.length; i++){
		if(pivot > arr[i]){
			swapIdx++;
			swap(arr, swapIdx, i);
		}
	}

	swap(arr, start, swapIdx);

	return swapIdx;
}
```

## quickSort

```js

```