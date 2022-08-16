# Tree

트리(Tree)란 그래프의 일종으로 한 노드에서 시작해 다른 노드들을 순회하여 자기 자신으로 돌아오는 사이클이 없는 연결 그래프이다. 이 때, 부모가 없는 루트 노드에서 시작하여 각 노드를 부모-자식 관계로 정의하고, 부모에서 자식으로 간선이 이어져 있는 유향 그래프이다. 또는 자료구조로써 트리는 비선형적이고, 각 노드에 값과 다른 노드에 대한 참조 목록을 저장한 계층적 데이터 구조로써 볼 수도 있다.

아래와 같이 컴퓨터 내의 폴더 구조가 가장 대표적인 트리의 예이다. 또한 트리는 루트 노드의 자식 또한 트리이고, 그 자식들 또한 트리가 되는 재귀적인 모습을 가진다는 특징이 있다.

<p align="center">
	<img src="https://programmer.group/images/article/a27323bfec47667970fdf5a9ac092fa4.jpg">
</p>

<br>

## 트리의 구성요소

<p align="center">
	<img src="https://miro.medium.com/max/700/0*reevDbBrtxor3Nyw.png">
</p>

- 트리(Tree) : 계층적인 자료를 표현하는 데 이용되는 비선형 자료 구조, 부모-자식 관계의 노드들로 이루어짐, 한 개 이상의 노드로 이루어진 유한 집합
- 노드(Node) : 트리의 구성요소 
  - 루트 노드(Root Node) : 트리 구조에서 가장 상위 노드
  - 부모 노드(Parent Node) : 루트 노드 방향으로 직접 연결된 노드
  - 자식 노드(Child Node) : 루트 노드 반대 방향으로 직접 연결된 노드
  - 형제 노드(Siblings Node) : 같은 부모를 갖는 노드들
  - 리프 노드(Leaf Node, Terminal Node, Internal Node) : 자식 노드가 없는 노드
  - 내부 노드(Internal Node) : 리프 노드가 아닌 노드
- 간선(Edge) : 노드와 노드를 연결하는 선
- 경로(Path) : 한 노드에서 다른 노드까지 갈 때 거치는 노드의 순서 집합
- 깊이(Depth) : 해당 노드에서 루트 노드까지 경로의 길이
- 높이(Height) : 가장 긴 깊이(리프 노드에서 루트 노드까지 가장 긴 경로의 길이)
- 차수(Degree) : 노드의 자식의 개수
- 포레스트(Forest) : 트리의 집합

<br>

## 트리의 정의

다음과 같은 트리의 정의는 모두 동치이다.

- 간선의 수가 최소인 연결 그래프이다.
- 싸이클이 없으면서, 간선의 수가 최대인 그래프이다.
- 간선을 하나만 추가해도 싸이클이 생기는, 싸이클이 없는 그래프이다.
- N개의 노드와 N-1개의 간선으로 이루어진 연결 그래프이다.
- 어떤 두 점을 잡아도, 두 노드를 연결하는 경로가 유일하게 하나 존재하는 그래프이다.
- 어떤 간선을 제거해도 연결 그래프가 아니게 되는 연결 그래프이다.

<br>

## 트리의 종류

### 1. 이진 트리(Binary Tree)

<p align="center">
	<img src="https://i1.faceprep.in/Companies-1/binary%20tree%20-%20types.png">
</p>

- 이진 트리(Binary Tree) : 모든 노드가 이진 트리인 두 개의 서브 트리를 가지는 트리. 즉 서브 트리의 모든 서브 트리도 이진 트리
  - 정 이진 트리(Full Binary Tree) : 모든 노드가 0개 또는 2개의 자식 노드만을 갖는 이진 트리
  - 완전 이진 트리(Complete Binary Tree) : 왼쪽 부터 빈틈없이 노드가 채워진 이진 트리
  - 포화 이진 트리(Perfect Binary Tree) : 모든 레벨이 꽉 찬 이진 트리
- 이진 트리는 다음과 같은 특징을 가짐
  - 높이가 h인 이진 트리는 최소 h개의 노드를 가지며 최대 $2^h-1$개의 노드를 가짐
  - n개의 노드를 가지는 이진트리의 높이는 최대 n이거나 최소 $log_2(n+1)$임

### 2. 이진 탐색 트리(Binary Search Tree)

- 이진 탐색 트리(Binary Search Tree) : 이진 트리 기반의 탐색을 위한 자료 구조
  - 왼쪽 서브트리 키들은 루트 키보다 작고, 오른쪽 서브트리 키들은 루트 키보다 큼
  - 왼쪽과 오른쪽 서브트리도 모두 이진 탐색 트리
  - 이진 탐색 트리에서 탐색, 삽입, 삭제 연산의 시간복잡도는 O(h) (h : 트리의 높의), 즉 트리가 극단적으로 편향(skew)되어 있는 경우 최악의 시간복잡도는 O(n)

<br>

## 트리의 순회

<p align="center">
	<img src="http://upload.wikimedia.org/wikipedia/commons/thumb/6/67/Sorted_binary_tree.svg/333px-Sorted_binary_tree.svg.png">
</p>

위와 같은 이진 트리가 주어졌다고 하자. 이진 트리의 경우 다음과 같이 주어진 트리를 탐색하는 방법에 따라 순회 방법이 구분된다.

- 전위 순회(Preorder Traversal) : 루트 노드 > 왼쪽 노드 > 오른쪽 노드 순으로 방문
- 중위 순회(Inorder Traversal) : 왼쪽 노드 > 루트 노드 > 오른쪽 노드 순으로 방문
- 후위 순회(Postorder Traversal) : 왼쪽 노드 > 오른쪽 노드 > 루트 노드 순으로 방문
- 레벨 순회(Levelorder Traversal) : 각 노드를 레벨 순서대로 방문

따라서 위와 같은 방법으로 주어진 그래프를 순회하면 다음과 같다

- 전위 순회(Preorder Traversal) : F > B > A > D > C > E > G > I > H
- 중위 순회(Inorder Traversal) : A > B > C > D > E > F> G > H > I
- 후위 순회 : A > C > E > D > B > H > I > G > F
- 레벨 순회 : F > B > G > A > D > I > C > E > H

각각의 순회는 다음과 같이 재귀 함수의 형태로 구현할 수 있다. 트리 순회와 관련된 다음과 같은 [트리 순회](https://www.acmicpc.net/problem/1991) 문제를 참고해보자. 

```python
# Tree의 순회 ; 전위순회, 중위순회, 후위순회

def preorder(node):
    
    if node != 0:
        yield node
        preorder(trees[node][0])
        preorder(trees[node][1])


def inorder(node):
    
    if node != 0:
        inorder(trees[node][0])
        yield node
        inorder(trees[node][1])
    

def postorder(node):
    
    if node != 0:
        postorder(trees[node][0])
        postorder(trees[node][1])
        yield node
```

<br>

## 예제

### 1. [트리](https://www.acmicpc.net/problem/4803)

어떠한 그래프가 주어졌을 때, 그래프가 트리인지 아닌지 혹은 트리(또는 포레스트)라면 몇 개의 트리로 구성되어 있는지 세는 문제이다. 먼저 dfs를 통해 재귀적으로 자식 노드를 탐색해나간다. 이 때, 해당 자식 노드가 방문한 적이 있다면 사이클이 존재하므로 해당 그래프는 트리가 아님을 알 수 있다. 모든 방문하지 않은 노드들에 대하여 dfs 탐색을 진행해나가면서 해당 그래프가 트리인지 아닌지를 판단하고, 트리라면 해당 dfs 방문이 끝날 때, cnt에 1을 더하여 트리의 개수를 센다. 

```python
# boj 4803 - 트리

def dfs(cur, bfr):
    
    global no_tree
    
    for nxt in graphs[cur]:
        if nxt != bfr:
            if visited[nxt]:
                no_tree = True
            else:
                visited[nxt] = True
                dfs(nxt, cur)


case = 0
while True:
    
    n, m = map(int, input().split())
    case += 1 
    
    if (n, m) == (0, 0):
        break
    
    graphs = [[] for _ in range(n+1)]
    for _ in range(m):
        a, b = map(int, input().split())
        graphs[a].append(b)
        graphs[b].append(a)
        
    cnt = 0
    visited = [False] * (n+1)
    for i in range(1, n+1):
        if not visited[i]:
            no_tree = False
            cnt += 1
            dfs(i, 0)
            if no_tree:
                cnt -= 1
    
    if cnt == 0:
        print(f'Case {case}: No trees.')
    elif cnt == 1:
        print(f'Case {case}: There is one tree.')
    else:
        print(f'Case {case}: A forest of {cnt} trees.')
```

### 2. [트리의 높이와 너비](https://www.acmicpc.net/problem/2250)

주어진 이진 트리에서 각 레벨 별 너비의 최댓값을 구하는 문제이다.

해당 문제에서 각 노드의 x좌표는 중위 순회하여 방문하는 노드의 순서와 같다. 따라서 dfs를 통해 중위 순회를 진행하고 각 노드를 방문할 때마다의 x 좌표를 저장한다. 이 때, 각 레벨 별로 가장 먼저 나오는 노드의 위치를 left에, 가장 나중에 나오는 노드의 위치를 right에 저장하여 레벨 별 노드의 위치의 차의 최댓값을 출력하도록 한다.

```python
# boj 2250 - 트리의 높이와 너비

def dfs(cur, depth):
    
    global x
    
    if cur == -1:
        x += 1
        return 1
    
    dfs(graphs[cur][0], depth+1)
    if not left[depth]:
        left[depth] = x
    right[depth] = x
    dfs(graphs[cur][1], depth+1)

    return x


n = int(input())

is_root = [True] * (n+1)
is_root[0] = False
graphs = [[] for _ in range(n+1)]
for _ in range(n):
    m, l, r = map(int, input().split())
    if l != -1:
        is_root[l] = False
    if r != -1:
        is_root[r] = False
    graphs[m] += [l, r]
    
root = is_root.index(True)

x = 0
left = [0] * (n+1)
right = [0] * (n+1)
dfs(root, 1)

max_width = -1
max_level = -1
for i in range(1, n+1):
    
    if (left[i], right[i]) == (0, 0):
        break
    
    width = right[i] - left[i] + 1
    if max_width < width:
        max_width = width
        max_level = i
        
print(max_level, max_width)
```

### 3. [트리의 순회](https://www.acmicpc.net/problem/2263)

n개의 정점을 갖는 주어진 이진 트리에서의 중위 순회와 후위 순회 결과가 주어졌을 때, 전위 순회한 결과를 출력하는 문제이다.

후위 순회하는 경우, 마지막의 방문하는 노드가 항상 해당 트리의 루트 노드임과 분할 정복을 이용한다. 먼저 후위 순회하는 마지막 수인 루트 노드를 기점으로 왼쪽 트리, 오른쪽 트리의 전위 순회 결과를 출력하도록 한다. 먼저 중위 순회의 인덱스를 pos에 저장한 후, 분할 정복 알고리즘을 이용하여 왼쪽 트리, 오른쪽 트리의 인덱스를 넘기면서 재귀적으로 구현한다. (이 때, 파이썬 기준 리스트를 인자로 넘기면 메모리 초과가 발생한다) 단, 이 경우 중위 순회에서 왼쪽 트리, 오른쪽 트리는 루트 노드를 기준으로 구분되지만, 후위 순회에서는 왼쪽 트리, 오른쪽 트리가 중위 순회에서 구한 왼쪽 트리의 개수 만큼의 인덱스를 통해서 구분됨을 주의 한다.

비슷한 방법으로 이진 트리에서 전위 순회와 중위 순회한 결과가 주어졌을 때, 후위 순회를 구하는 다음 문제([트리](https://www.acmicpc.net/problem/4256))와 이진 검색 트리에서 전위 순회한 결과가 주어졌을 때, 후위 순회한 결과를 구하는 다음 문제([이진 검색 트리](https://www.acmicpc.net/problem/5639))도 참고해본다.

```python
# boj 2263 - 트리의 순회

import sys
sys.setrecursionlimit(200000)    # n이 최대 100000이므로 이보다 큰 재귀횟수로 설정

def dq(i_s, i_e, p_s, p_e):
    
    if i_s >= i_e - 1:
        if i_s == i_e - 1:
            print(inorder[i_s], end=' ')
        return
    
    root = postorder[p_e-1]
    i_root = pos[root]
    
    print(root, end=' ')
    dq(i_s, i_root, p_s, p_s + i_root - i_s)
    dq(i_root+1, i_e, p_s + i_root - i_s, p_e-1)


n = int(input())
inorder = list(map(int, input().split()))
postorder = list(map(int, input().split()))

pos = [0]*(n+1)
for i in range(n):
    pos[inorder[i]] = i
dq(0, n, 0, n)

```

### 4. [트리의 지름](https://www.acmicpc.net/problem/1967)

주어진 트리의 두 노드 사이의 경로의 길이 중의 최댓값(=트리의 지름)을 구하는 문제이다. 즉, 가장 멀리 떨어져 있는 두 정점의 거리를 찾으면 된다. 이 경우, 루트(또는 임의의 정점)에서 가장 먼 거리의 정점을 하나 구한 후, 해당 정점에서 가장 먼 거리의 정점을 구하면 두 정점 사이의 거리가 트리의 지름이 된다.

단순히 루트에서 가장 멀리 떨어진 두 정점 사이의 거리가 트리의 지름이 되지 않음을 유의한다. (문제의 예시를 참고한다.)

```python
# boj 1967 - 트리의 지름

def dfs(cur, dist):
    
    global max_dist, max_pos
    
    if dist > max_dist:
        max_dist = dist
        max_pos = cur
    
    for nxt, n_dist in graphs[cur]:
        if not visited[nxt]:
            visited[nxt] = True
            dfs(nxt, dist+n_dist)


n = int(input())
graphs = [[] for _ in range(n+1)]
for _ in range(n-1):
    
    v, w, d = map(int, input().split())
    graphs[v].append((w, d))
    graphs[w].append((v, d))


max_dist = 0
max_pos = 0

visited = [False] * (n+1)
visited[1] = True
dfs(1, 0)

visited = [False] * (n+1)
visited[max_pos] = True
dfs(max_pos, 0)

print(max_dist)
```

<br>

### 참고 문제

<details>
<summary><u><font size="+1"><a href="https://www.acmicpc.net/problem/11725">[boj] 11725 - 트리의 부모 찾기 </a></font></u></summary>
\- 루트 노드가 주어진 트리에서 각 노드의 부모 노드를 찾는 문제 </br>
\- 루트 노드부터 dfs를 통해 방문하면, 현재 노드의 다음 방문 노드는 자식 노드가 됨을 이용 </br>
\- <a href="">[Python]</a>
</details>

<details>
<summary><u><font size="+1"><a href="https://www.acmicpc.net/problem/6416">[boj] 6416 - 트리인가? </a></font></u></summary>
\- 입력 받은 유향그래프가 트리인지 확인하는 문제 </br>
\- 해당 노드에서 나가는 간선 (outbound)과 들어오는 간선 (inbound)를 이용 </br>
\- 루트 노드를 제외한 모든 노드에 대하여 들어오는 간선이 1개인지(루트 노드만 들어오는 간선이 0개) 체크 </br>
\- <a href="">[Python]</a>
</details>

<details>
<summary><u><font size="+1"><a href="https://www.acmicpc.net/problem/9934">[boj] 9934 - 완전 이진 트리 </a></font></u></summary>
\- 완전 이진 트리에서 중위 순회한 결과가 주어질 때, 레벨 순회한 결과를 출력하는 문제 </br>
\- 각 레벨 별로, 2^k가 해당 레벨의 첫 번째 노드임을 이용 </br>
\- <a href="">[Python]</a>
</details>

<details>
<summary><u><font size="+1"><a href="https://www.acmicpc.net/problem/16437">[boj] 16437 - 양 구출 작전 </a></font></u></summary>
\- dfs 통해 트리를 탐색, 이 때 현재 노드까지의 양이 존재할때만 양의 수(양 +1, 늑대 -1)를 반환하면서 루트노드까지 도달 했을 때 총 양의 수의 합을 출력 </br>
\- <a href="">[Python]</a>
</details>

<details>
<summary><u><font size="+1"><a href="https://school.programmers.co.kr/learn/courses/30/lessons/92343">[programmers] 92334 - 양과 늑대 </a></font></u></summary>
\- 트리 구조에서 방문한 노드들의 양의 합이 늑대의 합보다 항상 크게 탐색하면서, 최대 양의 개수를 구하는 문제 </br>
\- dfs + 완전탐색을 통해 최대 양의 수를 업데이트 </br>
\- 이 때, 다음 방문할 수 있는 노드는 이전 방문 순서와 상관없이 이전 방문한 노드들의 자식 노드들이 됨 </br>
\- <a href="">[Python]</a>
</details>


