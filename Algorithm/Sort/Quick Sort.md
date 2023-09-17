<img width="100%" alt="image" src="https://wat-images.s3.ap-south-1.amazonaws.com/images/course/ci6ldqnqthum/Quick_Sort_0.png">
image source : https://wat-images.s3.ap-south-1.amazonaws.com/images/course/ci6ldqnqthum/Quick_Sort_0.png
# Principle
- 0개 혹은 1개의 원소를 가진 배열은 항상 정렬되어 있다는 사실을 이용한다.
- `pivot`으로 지정한 하나의 원소를 가지고, 정렬된 배열에서 `pivot`이 끝부분이어야하는 인덱스를 찾는다.

# Feature
- Worst Case가 되는 것을 최대한 억제하기 위해, 중앙값을 `pivot`으로 삼는 것으로 진행하는 방법도 있다.

# Time Complexity
- General, Best Case : O(n log n) - 정렬(comparisons per decomposition)이 O(n), 분할(decompositions)이 O(log n)이기 때문.
- Worst Case : O(n^2) - 정렬된 데이터에 대해서 `Quick Sort`를 진행할 경우, 정렬과 분할이 전부 O(n)이기 때문.

# Pseudo Code

- merge
- merge sort

# Implement

## merge

```js

```

## mergeSort

```js

```