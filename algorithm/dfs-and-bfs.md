# DFS & BFS

[그래프(Graph)]()란 정점(Vertex)과 간선(Edge)으로 이루어진 자료 구조를 말하며, 그래프의 탐색(Graph Traverse)이란 하나의 정점에서 시작하여 그래프의 모든 정점을 한 번씩만 방문하는 것 의미한다. 그래프를 탐색하는 방법은 크게 깊이 우선 탐색(DFS, Depth-First Search)와 너비 우선 탐색(BFS, Breadth-First Search)로 나눌 수 있다. 

<p align="center"><img src="https://i.stack.imgur.com/ByTKh.gif"></p>

<br>

## DFS

DFS는 루트 노드(혹은 임의의 노드)에서 시작하여 다음 분기로 넘어가기 전에 해당 분기를 완벽히 탐색하는 방법이다. 일반적으로 스택 또는 재귀함수를 이용하여 DFS를 구현한다.

일반적으로 코드 구현이 간단하고, 현재 진행 경로상의 노드들만 저장하면 되므로 메모리 사용이 비교적 적지만 해가 없는 경로가 깊을 경우 탐색시간이 오래 걸리고, 얻어진 해가 최단경로가 된다는 보장을 하지 못한다.

### 재귀함수를 이용한 DFS

재귀함수를 이용하여 DFS를 구현하는 경우, 재귀를 많이 호출한다면 재귀 제한 횟수로 인해 런타임 에러가 발생할 수도 있다.

```python
# DFS ; 재귀함수 형태의 알고리즘
# 인접리스트로 표현된 graph

graph = [[], [2, 3, 8], [1, 7], [1, 4, 5], [3, 5], [3, 4], [7], [2, 6, 8], [1, 7]]
visited = [False] * len(graph)

def dfs_recursive(graph, v, visited):
    visited[v] = True
    print(v, end=' ')
    for i in graph[v]:
        if not visited[i]:
            dfs_recursive(graph, i, visited)
            
dfs_recursive(graph, 1, visited)

>> Result ; 1 2 7 6 8 3 4 5
```

### 스택을 이용한 DFS

```python
# DFS ; stack 활용
# 인접리스트로 표현된 graph

graph = [[], [2, 3, 8], [1, 7], [1, 4, 5], [3, 5], [3, 4], [7], [2, 6, 8], [1, 7]]
visited = [False] * len(graph)

def dfs_stack(graph, v, visited):
    
    stack = [v]
     
    while stack:
        v = stack.pop()
        if not visited[v]:
            visited[v] = True
            print(v, end=' ')
            stack.extend(sorted(graph[v], reverse=True)) # 작은 순서대로 방문하기 위해 sorted 활용
            
dfs_stack(graph, 1, visited)

>> Result ; 1 2 7 6 8 3 4 5
```

## BFS

BFS는 루트 노드(혹은 임의의 노드)에서 시작하여 인접한 노드를 우선적으로 탐색하는 방법이다. 일반적으로 큐를 이용하여 BFS를 구현한다. 

해가 되는 경로가 여러 개인 경우에도 항상 최단경로임이 보장되므로 일반적으로 가중치가 없는 최단경로를 구할 때 BFS를 활용한다. 다만 해의 경로의 길이가 매우 긴 경우, 탐색 가지가 증가함에 따라 탐색해야 하는 노드가 많이 필요하므로 메모리 사용량이 비교적 크다.

### 큐를 이용한 BFS

```python
# BFS ; Queue 활용
# 인접리스트로 표현된 graph

graph = [[], [2, 3, 8], [1, 7], [1, 4, 5], [3, 5], [3, 4], [7], [2, 6, 8], [1, 7]]
visited = [False] * len(graph)

from collections import deque

def bfs(graph, start, visited):
    queue = deque([start])
    visited[start] = True
    while queue:
        v = queue.popleft()
        print(v, end=' ')
        for i in graph[v]:
            if not visited[i]:
                queue.append(i)
                visited[i] = True
                
bfs(graph, 1, visited)

>> Result ; 1 2 3 8 7 4 5 6
```

<br>

## DFS vs. BFS

- 인접 행렬로 그래프를 탐색하는 경우 하나의 정점을 탐색할 때 나머지 V개의 정점을 모두 방문해야 하므로 전체 시간복잡도는 O(V^2)이 된다. 반면 인접 리스트로 구현하는 경우, 각 정점에 연결되어 있는 간선의 개수만큼 탐색을 해야 하므로 시간복잡도는 O(V+E)이다. 그래프 탐색을 하는 경우 DFS, BFS 모두 인접행렬보다는 인접리스트를 이용하여 구현하는 것이 효율적이다.

- 일반적으로 구현은 DFS가 BFS보다 간단하지만 검색속도는 BFS가 DFS보다 빠르다.
- 단순히 모든 정점을 방문해야 하는 경우는 DFS와 BFS, 두 가지 방법 중 어느 것을 사용해도 상관없다.
- 경로의 특징을 저장해둬야 하는 경우(이동할 때마다 가중치가 추가 된다든가 이동 과정에서의 제약 사항이 존재하는 경우)는 DFS를, 최단거리를 구해야 하는 경우는 BFS를 활용한다.
- 검색 대상인 그래프가 큰 경우 DFS를, 그래프의 규모가 크지 않고 시작 지점으로부터 원하는 대상이 그리 멀지 않은 경우 BFS를 활용한다.

<br>

## 예제

### 1. DFS & BFS

```python
# 1. DFS 기본 예제 - 음료수 얼려 먹기
# 4 5
# 00110
# 00011
# 11111
# 00000
# >> 3

def dfs(x, y):

    if x < 0 or x >= n or y < 0 or y >= m:
        return False
    
    if graph[x][y] == 0:
        
        graph[x][y] = 1
        
        dfs(x-1, y)
        dfs(x, y-1)
        dfs(x+1, y)
        dfs(x, y+1)
        
        return True
    
    return False


n, m = map(int, input().split())
graph = [list(map(int, input())) for _ in range(n)]
    
answer = 0
for i in range(n):
    for j in range(m):
        if dfs(i, j):
            answer += 1

print(answer)


# 2. BFS 기본 예제 - 미로 탈출
# 5 6
# 101010
# 111111
# 000001
# 111111
# 111111
# >> 10

from collections import deque

def bfs(x, y):

    queue = deque()
    queue.append((x, y))
    
    while queue:
        
        x, y = queue.popleft()
        
        for i in range(4):
            
            nx = x + dx[i]
            ny = y + dy[i]
            
            if nx < 0 or nx >= n or ny < 0 or ny >= m:
                continue
            
            if graph[nx][ny] == 0:
                continue
                
            if graph[nx][ny] == 1:
                graph[nx][ny] = graph[x][y] + 1
                queue.append((nx, ny))
                
    return graph[n-1][m-1]


n, m = map(int, input().split())
graph = [list(map(int, input())) for _ in range(n)]

dx = [-1, 1, 0, 0]
dy = [0, 0, -1, 1]

print(bfs(0, 0))
```

**Reference**

- [(이코테 2021 강의 몰아보기) 3. DFS & BFS - YouTube](https://www.youtube.com/watch?v=7C9RgOcvkvo)

## 참고 문제

- [[boj] 1012 - 유기농 배추](https://www.acmicpc.net/problem/1012)
- [[boj] 2178 - 미로 탐색](https://www.acmicpc.net/problem/2178)
