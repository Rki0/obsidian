- A method for solving a complex problem by breaking it down into a collection of simpler subproblems, solving each of those subproblems just once, and storing their solutions

# Feature
- 많은 문제에 적용할 수 있는 개념은 아니지만, 이 개념을 적용할 수 있는 문제에 대해서는 그 효율이 기하급수적으로 증가한다.
- `Dynamic Programming`에서 `Dynamic`이란 시간에 따라 변화하는 문제를 표현하기 위한 단어로 쓰인 듯하다.

# Overlapping Subproblems
- A problem is said to have overlapping subproblems, if it can be broken down into subproblems which are reused several times.
- 몇 번이고 다시 사용될 수 있는 하위 문제로 쪼개질 수 있는 것을 `Overlapping Problems`라고 한다.

# Optimal Substructure
- A problem is said to have optimal substructure, if an optimal solution can be constructed from optimal solutions of its subproblems.
- 어떤 문제의 최적의 답이 그 하위 문제의 최적의 답을 통해 만들어질 수 있다면, 그 문제는 `Optimal Substructure`를 가지고 있다고 할 수 있다.
- 가령, 최단 경로를 구할 때, `A -> D`가 최단 경로라면, 그 하위 경로인 `A -> C`의 최단 경로는 `A -> D`에서의 `A -> C`와 동일할 것이고, 하위 경로인 `A -> B`의 최단 경로 또한 `A -> D`에서의 `A -> B`와 동일할 것이다.

# Using past knowledge!
- 값을 잘 기억해놓고, 동일한 연산이 나올 때 그 값을 다시 꺼내서 사용하자!
- 값을 기억하는 방법은 크게 두 가지로 나눌 수 있다.
## Memoization
- Storing the results of expensive function calls and returning the cached result when the same inputs occur again.
- Top-Down

## Tabulation