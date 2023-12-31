<img width="100%" alt="image" src="http://1.bp.blogspot.com/-YJDalyxz6XY/UFCYnd2_nBI/AAAAAAAAAB4/uewJpXgs9Mc/s640/Brute+Force.jpg">
image source : http://1.bp.blogspot.com/-YJDalyxz6XY/UFCYnd2_nBI/AAAAAAAAAB4/uewJpXgs9Mc/s640/Brute+Force.jpg
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
- `long`의 특정 위치부터 `short`를 순회하면서, 두 문자열이 일치하면 계속해서 진행하고, 일치하지 않으면 `break`를 하고 `long`의 다음 문자열부터 다시 시작하는 방식이다.
- `j`가 `short.length - 1`과 같아지면 `short`를 `long`에서 찾게 된 것이므로 `count`를 증가시킨다.

```js
function naiveStringSearch(long, short){
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