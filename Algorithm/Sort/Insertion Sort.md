<img width="100%" alt="image" src="https://prepinsta.com/wp-content/uploads/2020/05/Insertion-Sort-in-C.png">
image source : https://prepinsta.com/wp-content/uploads/2020/05/Insertion-Sort-in-C.png
# Principle
- 

# Feature
- 원소를 순회하면서 그 왼쪽을 정렬된 배열로 만들어가는 방법이다.
- `Selection Sort`, `Bubble Sort`보다 비교적 빠르다.
- 정렬을 건드리지 않고 새로 들어온 것의 자리만 찾아주면 되기 때문에 라이브, 스트리밍 방식으로 들어온 데이터를 즉시 정렬해야하는 경우에 좋다.

# Time Complexity
- General, Worst Case : O( n^2)
- Best Case : O(n) - 거의 정렬이 되어있거나, 정렬이 이미 되어있는 상태인 경우.

# Space Complexity
- O(1)

# Pseudo Code
1. Start by picking the second element in the array
2. Now compare the second element with the one before it and swap if necessary
3. Continue to the next element and if it is in the incorrect order, iterate through the sorted portion(i.e. the left side) to place the element in the correct place
4. Repeat until the array is sorted

# Implement
- `curr`을 기준으로 왼쪽에 있는 배열이 정렬되도록 만드는 것을 기억하자.

```js
function insertionSort(arr) {
	for(let i = 1; i < arr.length; i++){
		let curr = arr[i];
		let j = i - 1;

		while(j >= 0 && arr[j] > curr){
			arr[j + 1] = arr[j];
			j--;
		}

		arr[j + 1] = curr;
	}

	return arr;
}
```

<img width="100%" alt="image" src="https://github.com/Rki0/obsidian/assets/86224851/2f4c336e-7ded-4005-9ed2-2d0afa552379">
