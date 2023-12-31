<img width="100%" alt="image" src="https://www.w3resource.com/w3r_images/bubble-short.png">
image source : https://www.w3resource.com/w3r_images/bubble-short.png
# Principle
- 왼쪽부터 한 칸씩 이동하면서 이웃한 두 수를 비교해서 순서가 제대로 되어 있지 않으면 바꾸는 작업을 반복.
- 이 과정을 반복할 때마다 마지막 자리에 옮겨진 원소는 정렬이 끝날 때까지 그 자리를 유지한다.

# Feature
- 순회가 끝나기 전에 전부 정렬이 된다면 더 이상 정렬할 필요가 없으므로, 순회 도중 교환 여부를 체크해서 교환이 없으면 그대로 종료시키는 것으로 최적화가 가능하다.
- 두 수를 비교하는 총 횟수는 `n-1`부터 점점 줄어들며 누적합이 되어서 `n(n-1)/2`가 된다. 따라서 시간 복잡도는 `O(n^2)`.
- 반복을 거듭함에 따라 정렬해야할 요소 개수가 줄어든다.
- `Sinking Sort`라고도 하며, `sink`는 하단, 즉, 왼쪽 끝을 의미한다.

# Time Complexity
- General, Worst Case : O( n^2)
- Best Case : O(n) - 거의 정렬이 되어있거나, 정렬이 이미 되어있는 상태인 경우.

# Space Complexity
- O(1)

# Pseudo Code
1. Start looping with a variable called i the end of the array towards the beginning
2. Start an inner loop with a variable called j from the beginning until i-1
3. If `arr[j]` is greater than `arr[j+1]`, swap those who values
4. Return the sorted array

# Implement

## Unoptimized Version
- 한 번 순회할 때마다 연산 가장 뒤에 옮겨지는 값은 정렬이 끝난 상태이므로 `i - 1`까지만 연산한다.
- 현재 코드는 오름차순 정렬로 구현되어 있다.

```js
function swap(arr, idx1, idx2) {
	[arr[idx1], arr[idx2]] = [arr[idx2], arr[idx1]];
}

function bubbleSort(arr) {
	for(let i = arr.length - 1; i > 0; i--){
		for(let j = 0; j < i; j++){
			if(arr[j] > arr[j + 1]){
				swap(arr, j, j + 1);
			}
		}
	}

	return arr;
}
```

## Optimized Version
- 데이터가 거의 정렬이 된 상태이거나, 이미 정렬이 완료된 상태라면 `Bubble Sort`를 진행할 필요가 없다!
- 만약, 특정 순회에서 한번이라도 정렬이 발생하지 않는다면, 그 것은 모든 정렬이 완료된 것이므로 빠르게 연산을 종료한다.
- 현재 코드는 오름차순 정렬로 구현되어 있다.

```js
function swap(arr, idx1, idx2) {
	[arr[idx1], arr[idx2]] = [arr[idx2], arr[idx1]];
}

function bubbleSort(arr) {
	let noSwaps;

	for(let i = arr.length - 1; i > 0; i--){
		noSwaps = true;

		for(let j = 0; j < i; j++){
			if(arr[j] > arr[j + 1]){
				swap(arr, j, j + 1);

				noSwaps = false;
			}
		}

		if(noSwaps){
			break;
		}
	}

	return arr;
}
```