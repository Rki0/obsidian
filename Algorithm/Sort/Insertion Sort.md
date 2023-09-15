# Principle

# Feature
- 크기가 1인 이미 정렬된 배열에 하나씩 원소를 삽입하여 크기가 `n`인, 정렬된 배열을 만드는 알고리즘.
- ``

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
