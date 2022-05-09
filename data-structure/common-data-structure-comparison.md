# Data Structure

<p align="center"><img src="https://miro.medium.com/max/1400/1*2rKGJ6h1regwmfMcty3SLw.png"></p>

자료 구조(Data Structure)란 데이터 값의 모임 또는 그러한 데이터의 관계 등을 의미하며 자료에 대한 처리를 효율적으로 수행할 수 있도록 자료를 구분하여 표현한 것이다. 상황에 맞는 적절한 자료 구조의 선택은 자료를 더 효율적으로 저장하고, 체계적으로 관리할 수 있으며 이는 프로그램의 실행시간을 단축시키거나 메모리 사용량을 줄이는 등의 효과를 얻을 수 있다. 위의 그림에서는 대표적인 자료 구조인 배열(Array), 스택(Stack), 큐(Queue), 연결 리스트(Linked List), 해시 테이블(Hash Table), 그래프(Graph), 트리(Tree)를 살펴볼 수 있다. 이외에도 특수한 형태의 자료 구조로 힙(Heap), 이진 트리(Binary Tree), 세그먼트 트리(Segment Tree) 등이 있다.

<br>

## Linear vs. Non-Linear

자료구조는 크게 선형 자료 구조(Linear Data Structure)와 비선형 자료 구조(Non-Liniear Data Structure)로 구분할 수 있다. 선형 자료 구조는 말 그대로 데이터가 순차적으로 나열되어 있는 구조를 의미하며, 비선형 자료 구조는 선형이 아닌 특별한 형태로 데이터가 표현되어 있는 구조를 의미한다. 선형 자료 구조에서는 자료들 간의 관계가 1:1로 선형적으로 이어져 있고, 비선형 자료 구조에서는 자료들간의 관계가 1:n 또는 n:n의 비선형적으로 주어진다. 선형 자료 구조의 대표적인 예로 배열, 연결 리스트, 스택, 큐 등이 있으며, 비선형 자료 구조의 대표적인 예로 그래프, 트리 등이 있다.

- 선형 자료 구조 : 배열, 연결 리스트, 스택, 큐
- 비선형 자료 구조 : 그래프, 트리

<br>

## Time Complexity

자료 구조에서의 연산은 접근(Access : i번째 인덱스에 해당하는 값), 검색(Search : 특정 원소 검색), 삽입(Insertion : 특정 원소 삽입), 삭제(Deletion : 특정 원소 삭제)으로 구분할 수 있다. 대표적인 몇 가지 자료 구조에 대한 각 연산의 시간 복잡도는 다음과 같다. 

### 1. 배열(Array)

- Access : O(1)
  - 배열의 원소는 메모리에서 순차적으로 저장되어 있기 때문에(인덱싱이 되어 있음) 인덱스 번호로 배열의 원소에 접근 가능하다.
- Search : O(n) (정렬되어 있는 경우 O(log n))
  - 일반적인 배열에서의 검색은 첫번째 인덱스 부터 순차적으로 검색해야 한다.
  - 단, 배열이 정렬되어 있다면 이진 탐색을 통해 로그 시간의 시간복잡도를 가진다.
- Insertion, Deletion : O(n)
  - 최악의 케이스는 배열의 첫번째에 원소를 삽입 또는 삭제하는 경우, 뒤의 모든 원소들의 위치 변화(shifting)이 일어난다.

### 2. 연결 리스트(Linked List)

- Access, Search : O(n)
  - 특정 원소의 접근 또는 검색 시 Head Node(이중 연결 리스트일 경우 Tail Node에서도) 시작하여 순차적으로 진행해야 한다.
- Insertion, Deletion : O(1)
  - 단순히 특정 인덱스에 해당하는 노드에서의 삽입과 삭제는 해당 인덱스의 주변 노드들의 포인터만 수정하면 되므로 O(1)의 시간복잡도를 갖는다.
  - 다만, 일반적으로 추가하려는 노드에 접근하는 경우, 해당 위치까지 순차적으로 접근해야 하므로 (Head Node나 Tail Node가 아니라면) O(n)의 시간복잡도를 갖는다.

### 3. 스택(Stack), 큐(Queue)

- Search : O(n)
  - 특정한 원소를 찾기 위해 해당 원소가 나올 때 까지 스택 또는 큐에서 pop을 해야 한다.

- Insertion, Deletion : O(1)
  - 항상 스택의 경우 Top에서, 큐의 경우 삽입은 Rear, 삭제는 Front에서 데이터의 연산이 이루어지므로O(1)의 시간복잡도를 가진다.

### 4. 해시 테이블(Hash Table)

- Search, Insertion, Deletion : O(1) ~ O(n)
  - 검색, 저장, 삭제의 경우 모두 고유한 키의 해시 값을 통해 매칭되는 값을 찾고, 삽입하고, 삭제할 수 있으므로 (해시함수의 시간복잡도는 고려하지 않을 때) 시간복잡도는 O(1)이 된다.
  - 단, 최악의 케이스에 해시 충돌로 인해 모든 bucket에 해당하는 값을 살펴봐야하므로 시간복잡도 O(n)이 된다.

### 5. 이진 탐색 트리(Binary Search Tree)

- TBD

대표적인 자료 구조의 연산에 대한 시간 복잡도는 아래 그림과 같다.

<p align="center"><img src="https://3.bp.blogspot.com/-FKfFL6z0zcI/XlfPWDD-SGI/AAAAAAAAHPk/MQ1l_RovxXgTIJCTReag9FzJRA3TILiYACLcBGAsYHQ/s1600/7365ce00a403fa7605fd2058c80ea65599ce67a2.png"></p>

<br>

**Reference**

- [자료 구조 - 위키백과, 우리 모두의 백과사전 (wikipedia.org)](https://ko.wikipedia.org/wiki/자료_구조)

- [Data Structures - GeeksforGeeks](https://www.geeksforgeeks.org/data-structures/)

- [Big O cheat sheets (cooervo.github.io)](https://cooervo.github.io/Algorithms-DataStructures-BigONotation/index.html)