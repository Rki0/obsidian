
# Principle
- 

# Feature
- 크기가 N인 배열에서 가장 작은 값을 선택해서 맨 앞 원소와 자리를 변경하는 것을 반복하는 알고리즘.
- 순회 시 앞으로 옮긴 원소는 순회한 원소 중 가장 작은 원소이기 때문에 끝날 때까지 그 자리를 유지한다.
- 두 수를 비교해서 최소값을 찾는 작업을 몇 번 하는가에 따라서 시간 복잡도가 달라진다.
- 두 수를 비교하는 총 횟수는 `n-1`부터 점점 줄어들며, 누적합이 되어서 `n(n-1)/2`가 된다. 따라서 시간 복잡도는 `O(n^2)`.
- `Selection Sort`가 `Bubble Sort`보다 나은 경우는 단 하나, 스왑 횟수를 최소화해야하는 경우이다.

# Time Complexity
- General, Worst Case : O( n^2)
- Best Case : O(n) - 거의 정렬이 되어있거나, 정렬이 이미 되어있는 상태인 경우.

# Space Complexity
- O(1)

# Pseudo Code
1. Store the first element as the smallest value you've seen so far.
2. Compare this item to the next item in the array until you find a smaller number.
3. If a smaller number is found, designate that smaller number to be the new "minimum" and continue until the end of the array.
4. If the "minimum" is not the value(index) you initially began with, swap the two values.
5. Repeat this with the next element until the array is sorted.

# Implement
- 

```js
function swap(arr, idx1, idx2){
	[arr[idx1], arr[idx2]] = [arr[idx2], arr[idx1]];
}

function selectionSort(arr){
	for(let i = 0; i < arr.length; i++){
		let lowest = i;

		for(let j = i + 1; j < arr.length; j++){
			if(arr[lowest] > arr[j]){
				lowest = j;
			}
		}

		if(lowest !== i){
			swap(arr, lowest, i);
		}
	}

	return arr;
}
```

