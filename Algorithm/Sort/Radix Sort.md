<img width="100%" alt="image" src="https://www.alphacodingskills.com/cs/img/radix-sort-1.PNG">

<img width="100%" alt="image" src="https://www.alphacodingskills.com/cs/img/radix-sort-2.PNG">

<img width="100%" alt="image" src="https://www.alphacodingskills.com/cs/img/radix-sort-3.PNG">
image source : https://www.alphacodingskills.com/cs/pages/cs-program-for-radix-sort.php
# Principle
-  숫자의 크기가 숫자의 자릿수로 표현된다는 것을 활용한다.
- 즉, 자릿수가 많으면 더 큰 숫자라는 것을 활용한다는 것이다.

# Feature
- 정수를 정렬하기 위한 특별한 알고리즘이다.
- 이 알고리즘은 원소끼리 비교를 하지 않는다!

# Time Complexity
- General, Worst, Best Case : O(n * k) - k는 숫자의 길이(자릿수), n은 배열의 길이
- 이론적으로는 일반적인 `Comparision Sort(e.g. Bubble, Insertion, Selection, Merge, Quick Sort)`보다 빠른 정렬인데, `k`가 `log N`이 되느냐 아니냐에 따라 여러 가지 의견이 갈리고 있다.
****
# Space Complexity
- O(n + k)

# Pseudo Code
- In order to implement `Radix Sort`, it's helpful to build a few helper functions.
## helper functions
### getDigit
- Returns the digit in num at the given place value
### digitCount
- Returns the number of digits in num
### mostDigits
- Given an array of numbers, returns the number of digits in the largest in the list.
## merge sort
1. Define a function that accepts list of numbers
2. Figure out how many digits the largest number has
3. Loop from `k = 0` up to this largest number of digits
4. For each iteration of the loop
	1. Create buckets for each digit(0 to 9)
	2. Place each number in the corresponding bucket on its `k-th` digit
5. Replace our existing array with values in our buckets, starting with 0 and going up to 9.
6. Return list a the end!

# Implement
## getDigit
- `i`의 자릿수에 어떤 숫자가 들어있는지 알아내는 함수이다.
- `10^(알고자하는 자릿수)`으로 숫자를 나눈 뒤, 10으로 나눈 나머지 값을 얻으면 알고자하는 자릿수의 숫자를 얻을 수 있다.

```js
function getDigit(num, i){
	return Math.floor(Math.abs(num) / Math.pow(10, i)) % 10;
}
```

```
getDigit(12345, 0); // 5(1의 자리)

getDigit(12345, 1); // 4(10의 자리)

getDigit(12345, 2); // 3(100의 자리)

getDigit(12345, 3); // 2(1000의 자리)

getDigit(12345, 4); // 1(10000의 자리)

getDigit(12345, 5); // 0(100000의 자리)
```

## digitCount
- 숫자가 몇 자리 수인지 알아내는 함수
- `log10(0)`은 `-Infinity`이므로 예외처리를 해줘야한다.
- `log10()`와 `Math.floor()`를 사용해서 `num`의 `10^n` 중 `n`을 뽑아낸다.
- 1의 자리는 `n`은 0이므로, `+1`을 해서 표현한다.

```js
function digitCount(num){
	if(num === 0){
		return 1;
	}

	return Math.floor(Math.log10(Math.abs(num))) + 1;
}
```

```
digitCount(1); // 1

digitCount(25); // 2

digitCount(314); // 3
```

## mostDigits
- `Radix Sort`를 하기 위해 최대 몇 번의 순회를 해야하는지 알기 위해 주어진 숫자들 중 가장 큰 자릿수를 얻기 위한 함수
- `digitCount()`를 사용하여 각 원소의 자릿수를 비교한다.

```js
function mostDigits(nums){
	let maxDigits = 0;

	for(let i = 0; i < nums.length; i++){
		maxDigits = Math.max(maxDigits, digitCount(nums[i]));
	}

	return maxDigits;
}
```

```
mostDigits([1234, 56, 7]); // 4

mostDigits([1, 1, 11111, 1]); // 5

mostDigits([12, 34, 56, 78]); // 2
```

### radixSort
- `mostDigits()`를 통해 가장 긴 자릿수가 무엇인지 얻는다.
- 각각의 원소가 `k`번째 자리에 어떤 값을 가지고 있는지 얻고, 이에 따라 `digitBuckets`에 분류하여 저장한다.
- `nums`에 일차적으로 정렬된 배열인 `digitBuckets`를 넣어준다. 이 때, `digitBuckets`는 2차원 배열이므로 `spread operator`로 1차원 배열의 리스트로 만든 뒤, 빈 배열에 `concat`해서 내부 값들만 이어지도록 만들어준다.

```js
function radixSort(nums){
	let maxDigits = mostDigits(nums);

	for(let k = 0; k < maxDigits; k++){
		let digitBuckets = Array.from({length : 10}, () => []);

		for(let i = 0; i < nums.length; i++){
			let digit = getDigit(nums[i], k);
			digitBuckets[digit].push(nums[i]);
		}

		nums = [].concat(...digitBuckets);
	}

	return nums;
}
```