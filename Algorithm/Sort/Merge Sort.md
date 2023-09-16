<img width="100%" alt="image" src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/e6/Merge_sort_algorithm_diagram.svg/600px-Merge_sort_algorithm_diagram.svg.png">
image source : https://upload.wikimedia.org/wikipedia/commons/thumb/e/e6/Merge_sort_algorithm_diagram.svg/600px-Merge_sort_algorithm_diagram.svg.png
# Principle
- 배열을 0개 혹은 1개의 원소를 가질 때까지 최대한 잘게 쪼갠다.
- 0개 혹은 1개의 원소를 가진 배열은 항상 정렬되어 있다는 사실을 이용한다.
- 쪼개진 배열을 재조합하며 정렬된 상태를 만들어간다.

# Feature
- `Divide and Conquer`과 `Recursion`을 사용하는 정렬 방법이다.
- 더 작은 배열로 분할한 뒤, 각 부분 배열을 정렬하고 병합하는 과정을 재귀적으로 반복한다.
- 즉, `merging`과 `sorting`의 콤비네이션이라고 생각하면 된다.

# Time Complexity
- General, Worst, Best Case : O( n log n) - 정렬(comparisons per decomposition)이 O(n), 분할(decompositions)이 O(log n)이기 때문.

# Space Complexity
- O(n)

# Pseudo Code


# Implement

```js

```
