# Stack & Queue

자료구조 중 가장 기본적인 예시는 스택과 큐이다. 스택과 큐 모두 객체들의 집합으로써 데이터를 기록하는 선형 구조를 가진다. 이 때, 아래 그림과 같이 스택은 모든 원소의 삽입과 삭제가 리스트의 한쪽 끝에서만 이루어지고, 큐는 한쪽으로 데이터가 삽입되고 반대쪽으로 데이터가 삭제되는 구조를 가진다.

<p align="center"><img src="https://miro.medium.com/max/1400/1*zKnDkJpL-4GQ36kzrDiODQ.png"></p>

<br>

## 스택(Stack)

- 모든 원소의 삽입과 삭제가 리스트의 한쪽 끝(top)에서만 수행되는 제한 조건을 가진 선형 자료 구조

- 후입선출(LIFO ; Last-in First-out) 구조
- 기본 연산 : push, pop / top, isempty, isfull ~ O(1)
- stack이 가득 찬 상태에서 새로운 원소를 push할 경우 stack overflow, 반대로 stack이 빈 상태에서 pop 연산 수행시 stack underflow 발생

## 큐(Queue)

### 1. 선형 큐

- 한쪽(rear)으로 데이터가 삽입되고 반대(front) 방향으로 데이터가 삭제되는 선형 자료 구조

- 선입선출(FIFO ; First-in First-out) 구조
- 기본 연산 : enqueue, dequeue ~ O(1)

### 2. 원형 큐(Circular Queue)

<p align="center"><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbxYKyl%2Fbtq2NdjvNAk%2F67qIyyADkTzqGHenQagcVk%2Fimg.png"></p>

- 배열로 구현한 선형 큐를 개선하여 front와 rear가 이어져있는 큐를 원형 큐라 함

- 일반적으로 1차원 배열을 이용하여 구현하고, Head와 Tail 포인터를 이용하여 배열의 처음과 끝이 연결되어 있는 것으로 간주 (나머지 연산자 이용)
- 일반적으로 queue의 size-1만큼 사용하여 다음을 표현
  - front = rear 인 경우 공백 상태
  - front = (rear + 1) % QUEUE_SIZE 인 경우 포화 상태

### 3. 우선순위 큐(Priority Queue)

- 들어간 순서와 상관없이 우선순위가 높은 데이터를 먼저 삭제하는 자료 구조

- 일반적으로 힙(Heap) 이라는 자료구조로 구현하지만 배열(Array)나 연결 리스트(Linked List)로도 구현 가능
  - 일반적인 배열(또는 연결리스트)로 구현하는 경우 

    - 삽입의 경우 단순히 해당 원소를 배열에 넣으므로 시간복잡도 O(1)
    - 삭제의 경우 우선순위에 높은 원소를 해당 배열에서 검색해야 하고 이 경우 시간복잡도 O(n)

  - 정렬되어 있는 배열(또는 정렬되어 있는 연결리스트)로 구현하는 경우
    - 삽입의 경우 해당 원소를 우선순위에 따라 정렬하여 삽입해야 하므로, 해당 원소의 우선순위에 맞는 인덱스를 찾아 검색해야 하고 따라서 시간복잡도 O(n)
    - 삭제의 경우 해당 배열은 정렬되어 있으므로 가장 마지막 원소를 삭제하면 되므로 시간복잡도는 O(1)

  - 힙으로 구현하는 경우
    - 완전 이진 트리를 활용하여 삽입, 삭제 모두 시간복잡도 O(logn)에 해결 가능
    - 해당 내용은 heap 문서 참조


### 4. 덱(Deque ; Double-Ended Queue)

- 삽입과 삭제가 리스트의 양쪽 끝에서 모두 발생할 수 있는 선형 자료 구조

- 스택과 큐의 기능을 모두 지원

<br>

## Queue vs. Deque in Python

파이썬에서는 기본 제공 라이브러리 중 Queue의 queue 메서드를 통해서 queue를, collections의 deque 메서드를 통해서 deque을 구현 가능하다. 다만 단순히 자료구조로써 활용하는 경우 collections.deque을 활용하는 것이 좋다. Queue.queue는 멀티 스레드 환경에서 스레드 간의 안전한 통신이 가능할 수 있게끔 설계되었고, 반면 collections.deque은 자료구조로써 설계되었기 때문에 기본 연산이 더 빠르게 동작하기 때문이다. (Queue.queue에 비해 collections.deque이 약 20배 빠르다고 함) 

실제로 Queue.queue에는 put, get 이외에도 put_nowait, get_nowait 등의 멀티 스레드 환경에서 사용되는 메서드들이 존재한다. 

**Reference**

- [python - Queue.Queue vs. collections.deque - Stack Overflow](https://stackoverflow.com/questions/717148/queue-queue-vs-collections-deque)

<br>

## 참고 문제

<details>
<summary><u><font size="+1"><a href="https://www.acmicpc.net/problem/1655">[boj] 1655 - 가운데를 말해요</a></font></u></summary>
  
\- 2개의 힙(max heap, min heap)을 이용 </br>
\- max_heap과 min_heap에 번갈아 가면서 숫자를 넣으면 가운데 숫자는 max_heap의 첫번째 원소 </br>
\- <a href="https://github.com/cgvvxx/PS/blob/master/ps/%EC%8A%A4%ED%83%9D%2C%20%ED%81%90/058_B_1655.py">[Python]</a>
</details>

<details>
<summary><u><font size="+1"><a href="https://www.acmicpc.net/problem/13334">[boj] 13334 - 철로</a></font></u></summary>
  
\- 끝나는 시간 기준으로 정렬 후 시작 시간을 담는 heap을 만들어 주어진 경우가 하나의 철로에 포함되는지 안되는지 체크 </br>
\- 힙을 이용하면 시간복잡도 O(nlogn) </br>
\- <a href="https://github.com/cgvvxx/PS/blob/master/ps/%EC%8A%A4%ED%83%9D%2C%20%ED%81%90/136_B_13334.py">[Python]</a>
</details>
