<img width="100%" alt="image" src="https://media.geeksforgeeks.org/wp-content/uploads/20221124153129/Treedatastructure.png">
image source : https://media.geeksforgeeks.org/wp-content/uploads/20221124153129/Treedatastructure.png
# Terminology
- 노드(node) : 트리를 구성하는 기본 단위. 데이터와 다른 노드를 가리키는 포인터를 포함한다.
- 루트(root) : 트리의 최상위 노드. 이 노드에서 모든 트리 순회가 시작된다.
- 리프(leaf) : 자식 노드가 없는 노드. Tree가 나무라면 리프 노드는 나뭇잎이다.
- 레벨(level) : 트리에서 같은 깊이에 있는 노드들의 집합. 예를들어, 루트 노드는 보통 레벨 1(또는 레벨 0)에 있고, 그 자식 노드들은 레벨 2에 있다.
- 서브 트리(subTree) : 특정 노드와 그 노드의 모든 자손 노드로 구성된 트리. 트리 내에 존재하는 다양한 크기의 트리들을 말한다.
- 자식(child) : 특정 노드의 하위 노드로 바로 연결되어 있는 노드를 말한다.
- 부모(parent) : 자식 노드를 가지고 있는 노드(물론 가지고 있지 않을 수도 있다.). 루트 노드를 제외한 노드는 반드시 1명의 부모를 갖는다.
- 형제(sibling) : 같은 부모를 가지는 자식 노드들을 말한다.
- 간선(edge) : 두 노드 간의 연결을 말한다.
- 깊이(depth) : 특정 노드에서 루트 노드까지 도달하기 위한 간선의 수.
- 높이(height) : 특정 노드에서 가장 멀리 떨어진 리프 노드까지의 "최장 경로"의 길이. 즉, 주어진 노드로부터 가장 먼 리프 노드까지 도달하기 위해 거쳐야하는 간선의 수. 참고로, 특정 노드의 그 하위 트리 중에서 리프 노드로 가는 것을 의미한다(트리를 거슬러 올라가지는 않기 때문).

# Types of Tree
[[Binary Search Tree]]
[[Balanced BST]]
[[AVL Tree]]
[[Red-Black Tree]]
[[B-Tree]]
[[B+Tree]]

