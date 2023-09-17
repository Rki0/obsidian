<img width="100%" alt="image" src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/e6/Merge_sort_algorithm_diagram.svg/600px-Merge_sort_algorithm_diagram.svg.png">
image source : https://upload.wikimedia.org/wikipedia/commons/thumb/e/e6/Merge_sort_algorithm_diagram.svg/600px-Merge_sort_algorithm_diagram.svg.png
# Principle
-  숫자의 크기가 숫자의 자릿수로 표현된다는 것을 활용한다.
- 즉, 자릿수가 많으면 더 큰 숫자라는 것을 활용한다는 것이다.
# Feature
- 정수를 정렬하기 위한 특별한 알고리즘이다.
- 이 알고리즘은 원소끼리 비교를 하지 않는다!

# Time Complexity
- General, Worst, Best Case : O(nk) - k는 숫자의 길이(자릿수), n은 배열의 길이

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

## merge sort


# Implement
- 
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
- `n`은 0부터 시

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