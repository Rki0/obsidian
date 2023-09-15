# Principle

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
1. Start looping with a variable called i the end of the array towards the beginning
2. Start an inner loop with a variable called j from the beginning until i-1
3. If `arr[j]` is greater than `arr[j+1]`, swap those who values
4. Return the sorted array

# Implement
