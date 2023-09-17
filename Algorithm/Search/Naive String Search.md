# Feature
- 긴 문자열에서 부분 문자열(substring)을 탐색하는 알고리즘

# Pseudo Code
1. Loop over the longer string
2. Loop over the shorter string
3. If the characters don't match, break out of the inner loop
4. If the characters do match, keep going
5. If you complete the inner loop and find a match, increment the count of the matches
6. Return the count

# Implement
- `long`은 긴 문자열, `short`는 검색하고자 하는 문자열을 의미한다.
- `long`의 특정 위치부터 `short`를 순회하면서, 두 문자열이 일치하면 `count`

```js
function naiveSearch(long, short){
	let count = 0;

	for(let i = 0; i < long.length; i++){
		for(let j = 0; j < short.length; j++){
			if(long[i + j] !== short[j]){
				break;
			}

			if(j === short.length - 1){
				count++;
			}
		}
	}

	return count;
}
```