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
- General, Worst, Best Case : O( n log n) - 정렬(comparisons per decomposition)이 O(n), 분할(decompositions)이 O(log n)이기 때문.

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

# Implement

```js

```
