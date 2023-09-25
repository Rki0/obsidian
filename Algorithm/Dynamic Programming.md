- A method for solving a complex problem by breaking it down into a collection of simpler subproblems, solving each of those subproblems just once, and storing their solutions

# Feature
- 많은 문제에 적용할 수 있는 개념은 아니지만, 이 개념을 적용할 수 있는 문제에 대해서는 그 효율이 기하급수적으로 증가한다.
- `Dynamic Programming`에서 `Dynamic`이란 시간에 따라 변화하는 문제를 표현하기 위한 단어로 쓰인 듯하다.

# Overlapping Subproblems
- A problem is said to have overlapping subproblems, if it can be broken down into subproblems which are reused several times.
- 몇 번이고 다시 사용될 수 있는 하위 문제로 쪼개질 수 있는 것을 `Overlapping Problems`라고 한다.

# Optimal Substructure
- A problem is said to have optimal substructure, if an optimal solution can be constructed from optimal solutions of its subproblems.
- 어떤 문제가 하위 문제의 최적의 해답을 통해 