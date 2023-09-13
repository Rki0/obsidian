
- [[Singly Linked List]]
- [[Doubly Linked List]]
- [[Circular Linked List]]

# Pros and Cons
- 배열과 비교했을 때, 삽입/삭제 연산에 효율적인 자료구조.
- 삽입/삭제가 잦은 알고리즘에 사용하면 좋다.
- 원소가 추가될 때마다 공간을 할당받아 비연속적인 메모리 공간에 추가한다.(배열은 주소가 연속적이다.)
- 탐색을 해야하는 경우에는 배열이 더 낫다. 배열은 인덱스를 통해 O(1)의 시간 복잡도로 탐색이 가능하기 때문.

# Time Complexity
- Access : O(n) - 순차적으로 node를 따라가기 때문.
- Searching : O(n) - 순차적으로 node를 따라가기 때문.
- Insertion
	- 맨 앞에 삽입 : O(1) - 가장 앞의 node에 연결하기 때문에, head의 참조만 바꾸면 된다.
	- 중간, 맨 끝에 삽입 : O(n) - 삽입할 위치를 찾아야하기 때문에.
- Removal
	- 맨 앞을 제거 : O(1) - 가장 앞의 node를 제거하기 때문에, head의 참조만 바꾸면 된다.
	- 중간, 맨 끝을 제거 : O(n) - 삭제하기 위한 node를 찾아야하기 때문에.

# How can I improve Time Complexity?
## Improve Time Complexity of Insertion
맨 끝에 삽입하는 경우 O(n)의 시간 복잡도를 가지는데, 이를 O(1)으로 개선할 수 있다!



# Related Algorithm

- [[Floyd's ...(토끼 거북)]]

