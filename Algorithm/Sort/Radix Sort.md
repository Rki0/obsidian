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

### mostDigits

## merge sort


# Implement
- 
## getDigit
- `i`의 자릿수에 어떤 숫자가 들어있는지 알아내는 함수이다.
- 

```js
function getDigit(num, i){
	return Math.floor(Math.abs(num) / Math.pow(10, i)) % 10;
}
```

```
// getDigit(12345, 0); // 5(1의 자리)

// getDigit(12345, 1); // 4(10의 자리)

// getDigit(12345, 2); // 3(100의 자리)

// getDigit(12345, 3); // 2(1000의 자리)

// getDigit(12345, 4); // 1(10000의 자리)

// getDigit(12345, 5); // 0(100000의 자리)
```
## mergeSort
- 

```js

```