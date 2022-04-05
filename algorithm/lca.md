# LCA (Lowest Common Ancestor)

LCA 알고리즘은 트리에서 주어진 두 노드 u, v에서 (자기 자신을 포함한) 가장 가까운 공통 조상을 찾는 알고리즘을 말한다. 예를 들어 다음과 같이 트리가 주어져 있다고 하고 LCA(u, v)는 노드 u와 v의 최소 공통 조상을 의미한다고 하자. 이 때 LCA(4, 6) = 1이다. 또한 만약 u과 v의 조상이라면 LCA(u, v) = u임을 알 수 있다.

**트리에서 두 노드 사이의 최단 경로**를 찾을 때 활용 가능하다. 예를 들어, LCA(u, v) = w라면 최단 경로는 u-w-v 형태로 주어짐을 알 수 있다.

![img](https://algo-logic.info/wp-content/uploads/2020/03/tree-2.png)

<br>

# LCA 알고리즘 동작 과정

## 1. dp를 활용한 LCA

*인접리스트로 주어진 트리* 가 주어지고 *루트 노드는 1* 이라고 하자. lca의 기본적인 구현 방식은 다음과 같다.

1. 두 정점의 높이(깊이)를 맞추어 준 후
2. 같은 깊이의 두 정점에서 동시에 조상을 탐색하면서 LCA를 탐색

단순히 높이를 맞춘 두 정점으로부터 선형적으로 부모 노드를 탐색한다면 전체 시간복잡도는 O(depth)가 된다. 최악의 경우, 트리가 불균형하게 이루어져 있다면 전체 노드를 탐색해야 하므로 시간복잡도는 O(n)이 된다. 이 때, 2의 과정을 이분 탐색을 활용하면 log 시간 내에 구할 수 있다.

먼저 부모 노드를 기록하는 2차원 배열 parents를 다음과 같이 정의한다.

``` parents[n][k] : 노드 n에서 2^k번째 조상 노드의 번호```

이 때 ``` parents[n][k] = parents[parents[n][k-1]][k-1] ``` 와 같은 등식이 성립한다.

즉, 노드 n에서 2^k번째 조상 노드는 노드 n에서 2^(k-1)번째 조상 노드의 2^(k-1)번째 조상 노드와 같다. 이를 이용하여 먼저 parents 배열에 대한 처리를 시행한다. 코드는 다음과 같다.

``` python
visited = [False] * (n+1) # n은 전체 노드의 개수, 각 노드는 1부터 n까지 번호로 주어지는 경우
d = [0] * (n+1) # 각 노드의 깊이

def dfs(u, depth): # 바로 위의 부모 노드만 기록
    visited[u] = True
    d[u] = depth
    for v in graphs[u]: # 그래프는 인접 리스트로 주어지는 경우
        if not visited[v]:
            parents[v][0] = u # 노드 v의 부모 노드는 parents[v][0]
            dfs(v, depth + 1)
    
def set_parent():
    
    dfs(1, 0) # 루트 노드는 1번으로 주어지는 경우
    
    for j in range(1, k): # 이 때 k는 log(n)의 올림 값
        for i in range(1, n+1):
            parents[i][j] = parents[parents[i][j-1]][j-1]
```

다음으로 두 정점의 높이를 맞추어주어야 한다. 

예를 들어서 깊이가 4인 노드 u와 깊이가 14인 노드 v가 있다고 하자. 두 노드의 깊이 차이는 10 = 1010(2) 이다. 이 때, 노드 u에서 2^1, 2^3번째 조상을 따라가면 log 시간 내에 두 정점의 높이를 맞출 수 있다. 코드는 다음과 같다.

``` python
if d[u] > d[v]: 
    u, v = v, u # v의 깊이가 u의 깊이보다 깊도록 설정

diff = d[v] - d[u]
for i in range(k):
    if diff & 1:
        v = parents[v][i]
        diff >>= 1
```

위의 과정을 통해 두 정점의 높이는 같아졌다. 즉 d[u] = d[v]인 상황이다. 이 때 서로 2^k번째 조상을 바라보면서 조상이 같아질 때까지 노드를 타고 올라가면 된다. 만약 ```parents[u][k] != parents[v][k]```라면 u, v의 LCA는 최소 2^k번째 조상보다는 멀리 떨어져 있음을 알 수 있다. 따라서 k를 큰 값부터 1씩 줄여가면서 두 노드의 2^k번째 조상이 다를 때, u와 v를 각각 2^k번째 조상으로 올려주면서 반복문을 진행하면 된다. 코드는 다음과 같다.

```python
for i in range(k-1, -1, -1):
    if parents[u][i] != parents[v][i]: # 두 노드의 2^i번째 조상이 다르면 u와 v를 각각 2^i번째 조상으로 업데이트
        u = parents[u][i]
        v = parents[v][i]
```

이와 같은 방법을 통해서 노드의 개수가 N인 트리에서 두 노드의 LCA를 시간복잡도 O(logN)으로 구할 수 있게 된다.

만약 M개의 쿼리문이 존재한다면 시간복잡도 O(MlogN)으로 해결할 수 있다.

### 예제

백준 11438 - LCA2 문제에 대한 예제 코드는 다음과 같다.

``` python
# boj 11438의 경우 pypy로 제출해야 통과

import sys
input = sys.stdin.readline
sys.setrecursionlimit(100000)


def dfs(u, depth):
    visited[u] = True
    d[u] = depth
    for v in graphs[u]: # 그래프는 인접 리스트로 주어지는 경우
        if not visited[v]:
            parents[v][0] = u # 노드 v의 부모 노드는 parents[v][0]
            dfs(v, depth + 1)
    
def set_parent():
    
    dfs(1, 0) # 루트 노드는 1번으로 주어지는 경우
    
    for j in range(1, k): # 이 때 k는 log(n)의 올림 값
        for i in range(1, n+1):
            parents[i][j] = parents[parents[i][j-1]][j-1]
            
def lca(u, v):
    
    if d[u] > d[v]: 
        u, v = v, u # v의 깊이가 u의 깊이보다 깊도록 설정

    diff = d[v] - d[u]
    for i in range(k):
        if diff & 1:
            v = parents[v][i]
        diff >>= 1
            
    if u == v:
        return u

    for i in range(k-1, -1, -1):
        if parents[u][i] != parents[v][i]: # 두 노드의 2^i번째 조상이 다르면 u와 v를 각각 2^i번째 조상으로 업데이트
            u = parents[u][i]
            v = parents[v][i]

    return parents[u][0]


n = int(input())
graphs = [[] for _ in range(n+1)]
for _ in range(n-1):
    a, b = map(int, input().split())
    graphs[a].append(b)
    graphs[b].append(a)

k = 21
visited = [False] * (n+1) # n은 전체 노드의 개수, 각 노드는 1부터 n까지 번호로 주어지는 경우
parents = [[0] * k for _ in range(n+1)]
d = [0] * (n+1) # 각 노드의 깊이

set_parent()

m = int(input())
for _ in range(m):
    u, v = map(int, input().split())
    print(lca(u, v))
```

**References**

- [최소 공통 조상(Lowest Common Ancestor, LCA) 알고리즘 10분 정복 - YouTube](https://www.youtube.com/watch?v=O895NbxirM8)

<br>

## 2. 세그먼트 트리를 활용한 lca

TBD

<br>

## 3. 참고 문제

- [[boj] 11438 - LCA 2](https://www.acmicpc.net/problem/11438)

- [[boj] 1761 - 정점들의 거리](https://www.acmicpc.net/problem/1761)

- [[boj] 3176 - 도로 네트워크](https://www.acmicpc.net/problem/3176)

- [[boj] 1396 -  크루스칼의 공](https://www.acmicpc.net/problem/1396)
