
- [[Singly Linked List]]
- [[Doubly Linked List]]
- [[Circular Linked List]]

# Pros and Cons
- 배열과 비교했을 때, 삽입/삭제 연산에 효율적인 자료구조.
- 삽입/삭제가 잦은 알고리즘에 사용하면 좋다.
- 원소가 추가될 때마다 공간을 할당받아 비연속적인 메모리 공간에 추가한다.(배열은 주소가 연속적이다.)

# Time Complexity
- Access : O(n) - 순차적으로 node를 따라가기 때문.
- Searching : O(n) - 순차적으로 node를 따라가기 때문.
- Insertion : O(1) - 맨 앞에 삽입하는 경우. 가장 앞의 node에 연결하기 때문에, head의 참조만 바꾸면 된다.
	- 맨 앞에 삽입 : O(1) - 가장 앞의 node에 연결하기 때문에, head의 참조만 바꾸면 된다.
	- 중간, 맨 끝에 삽입 : O(n) - 삽입할 위치를 찾아야하기 때문에.
- Removal : 

# Related Algorithm

- [[Floyd's ...(토끼 거북)]]

