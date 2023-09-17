<img width="100%" alt="image" src="https://sp-ao.shortpixel.ai/client/to_webp,q_glossy,ret_img,w_1333/https://learncodingfast.com/wp-content/uploads/2020/11/binary-search.png">
image source : https://sp-ao.shortpixel.ai/client/to_webp,q_glossy,ret_img,w_1333/https://learncodingfast.com/wp-content/uploads/2020/11/binary-search.png
# Feature
- 정렬된 데이터에 대해서 사용 가능한 검색 알고리즘이다.
- `Divide and Conquer` 개념을 사용한다.

# Time Complexity
- Worst, Average Case : O(log n)
- Best Case : O(1)

# Pseudo Code
1. Create a left pointer at the start of the array, and a right pointer at the end of the array
2. While the left pointer comes before the right pointer
	1. Create a pointer in the middle
	2. If you find the value you want, return the index
	3. If the value is too small, move the left pointer up
	4. If the value is too large, move the right pointer down
3. If you never find the value, return -1.

# Implement

```js
function binarySearch(arr, elem){
	let start = 0;
	let end = arr.length - 1;
	let middle = Math.floor((start + end) / 2);

	while(arr[middle] !== elem && start <= end){
		if(elem < arr[middle]){
			end = middle - 1;
		} else {
			start = middle + 1;
		}

		middle = Math.floor((start + end) / 2);
	}

	if(arr[middle] === elem){
		return middle;
	}

	return -1;
}
```