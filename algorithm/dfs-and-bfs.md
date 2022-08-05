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

### 1. [단지번호붙이기](https://www.acmicpc.net/problem/2667)

2차원 격자모양의 그래프에서 서로 다른 1의 개수(전체 단지의 수)와 서로 다른 1을 이루고 있는 숫자의 개수(각 단지에 속하는 집의 수)를 구하는 문제이다. dfs & bfs의 가장 기본적인 형태의 문제이고 문제 자체의 케이스가 크지 않아서 재귀함수를 이용한 dfs, 스택을 이용한 dfs, 큐를 이용한 bfs 어떤 형태로 풀어도 큰 차이가 나지 않는다.

```python
# 1. 재귀함수를 이용한 DFS

def dfs(x, y):
    
    global area
    
    if x < 0 or y < 0 or x >= n or y >= n:
        return
    
    if visited[x][y]:
        return
    
    if graph[x][y]:
        
        area += 1
        visited[x][y] = True
        
        dfs(x-1, y)
        dfs(x, y-1)
        dfs(x+1, y)
        dfs(x, y+1)


n = int(input())
graph = [list(map(int, list(input()))) for _ in range(n)]

visited = [[False] * n for _ in range(n)]
areas = []

for i in range(n):
    for j in range(n):
        if graph[i][j] and not visited[i][j]:
            area = 0
            dfs(i, j)
            areas.append(area)

areas.sort()
print(len(areas))
for a in areas:
    print(a)
```

```python
# 2. 스택을 이용한 DFS

def dfs(x, y):
    
    stack = []
    stack.append((x, y))
    area = 1
    
    while stack:
        
        x, y = stack.pop()
    
        for i in range(4):

            nx = x + dx[i]
            ny = y + dy[i]

            if nx < 0 or ny < 0 or nx >= n or ny >= n:
                continue

            if visited[nx][ny]:
                continue
                
            if graph[nx][ny]:
                visited[nx][ny] = True
                stack.append((nx, ny))
                area += 1
                
    return area


n = int(input())
graph = [list(map(int, list(input()))) for _ in range(n)]

dx = [1, -1, 0, 0]
dy = [0, 0, 1, -1]
visited = [[False] * n for _ in range(n)]
areas = []

for i in range(n):
    for j in range(n):
        if graph[i][j] and not visited[i][j]:
            visited[i][j] = True
            areas.append(dfs(i, j))

areas.sort()
print(len(areas))
for a in areas:
    print(a)
```

```python
# 3. 큐를 이용한 BFS

from collections import deque

def bfs(x, y):
    
    queue = deque()
    queue.append((x, y))
    area = 1
    
    while queue:
        
        x, y = queue.popleft()
    
        for i in range(4):

            nx = x + dx[i]
            ny = y + dy[i]

            if nx < 0 or ny < 0 or nx >= n or ny >= n:
                continue

            if visited[nx][ny]:
                continue
                
            if graph[nx][ny]:
                visited[nx][ny] = True
                queue.append((nx, ny))
                area += 1
    
    return area


n = int(input())
graph = [list(map(int, list(input()))) for _ in range(n)]

dx = [1, -1, 0, 0]
dy = [0, 0, 1, -1]
visited = [[False] * n for _ in range(n)]
areas = []

for i in range(n):
    for j in range(n):
        if graph[i][j] and not visited[i][j]:
            visited[i][j] = True
            areas.append(bfs(i, j))

areas.sort()
print(len(areas))
for a in areas:
    print(a)
```

### 2. [벽 부수고 이동하기](https://www.acmicpc.net/problem/2206)

기본적인 2차원 평면으로 주어진 그래프를 이용한 미로 탐색에 벽을 하나 부실 수 있다는 조건이 추가 되었다. 

기본적인 미로 탐색과 같이 bfs를 이용하고, 추가적으로 벽을 부술 수 있는 조건을 기록하기 위하여 visited를 3차원 배열로 만들어, 2차원 평면으로 주어진 그래프에서 벽을 뚫지 않고 해당 지점까지의 최단 경로 횟수와 벽을 뚫고 해당 지점까지의 최단 경로 횟수를 각각 저장하여 해결할 수 있다.

추가로 다음과 같은 시리즈의 문제들도 참고하면 좋다.

- [14442번: 벽 부수고 이동하기 2](https://www.acmicpc.net/problem/14442) : 벽을 최대 k개까지 부실 수 있음
- [16933번: 벽 부수고 이동하기 3](https://www.acmicpc.net/problem/16933) : 벽을 최대 k개까지 부실 수 있음 + 낮밤이 존재하여 낮에만 벽을 부실 수 있음
- [16946번: 벽 부수고 이동하기 4](https://www.acmicpc.net/problem/16946) : 각각의 벽의 위치에서 벽을 부수고 이동할 수 있는 최대 개수를 카운트

```python
# boj 2206 - 벽 부수고 이동하기

from collections import deque

def bfs():
    
    queue = deque()
    queue.append((0, 0, 0))
    visited[0][0][0] = 1
    
    while queue:
        
        x, y, z = queue.popleft()
        
        if (x, y) == (n-1, m-1):
            return visited[x][y][z]
        
        for i in range(4):
            
            nx = x + dx[i]
            ny = y + dy[i]
            
            if nx < 0 or ny < 0 or nx >= n or ny >= m:
                continue
                
            if visited[nx][ny][z] == 0:
                
                if graphs[nx][ny] == 0:
                    queue.append((nx, ny, z))
                    visited[nx][ny][z] = visited[x][y][z] + 1
                    
                if z == 0 and graphs[nx][ny] == 1:
                    queue.append((nx, ny, 1))
                    visited[nx][ny][1] = visited[x][y][z] + 1
                    
    return -1


n, m = map(int, input().split())
graphs = [list(map(int, list(input()))) for _ in range(n)]

visited = [[[0] * 2 for _ in range(m)] for _ in range(n)]
dx = [1, 0, -1, 0]
dy = [0, -1, 0, 1]
```



## 참고 문제

<details>
<summary><u><font size="+1"><a href="https://www.acmicpc.net/problem/2178">[boj] 2178 - 미로 탐색</a></font></u></summary>
\- 가장 기본적인 bfs 문제 미로 탐색 </br>
\- bfs를 통해 주변을 탐색해나가면서 2차원 평면으로 주어진 그래프를 출발지점에서 도착지점까지 탐색해나감</br>
\- <a href="https://github.com/cgvvxx/PS/blob/master/ps/DFS%2C%20BFS/041_B_2178.py">[Python]</a>
</details>

<details>
<summary><u><font size="+1"><a href="https://www.acmicpc.net/problem/6593">[boj] 6593 - 상범 빌딩</a></font></u></summary>

\- 기본적인 형태의 bfs + 3차원 그래프 미로 탐색</br>
\- <a href="https://github.com/cgvvxx/PS/blob/master/boj/boj_6593.py">[Python]</a>
</details>

<details>
<summary><u><font size="+1"><a href="https://www.acmicpc.net/problem/7562">[boj] 7562 - 나이트의 이동</a></font></u></summary>
\- 기본적인 형태의 bfs + 이동을 상하좌우가 아닌 나이트의 이동과 같이 8개의 방향으로 탐색 </br>
\- <a href="https://github.com/cgvvxx/PS/blob/master/ps/DFS%2C%20BFS/198_B_7562.py">[Python]</a>
</details>

<details>
<summary><u><font size="+1"><a href="https://www.acmicpc.net/problem/11724">[boj] 11724 - 연결 요소의 개수</a></font></u></summary>

\- 재귀함수 형태의 dfs를 이용 </br>
\- 해당 그래프를 연결리스트로 저장한 후, 각 연결리스트를 dfs로 돌면서 방문체크, 더 이상 dfs를 통해 방문할 수 없을때 count에 1을 더하여 모두 방문하였을 때의 count = 연결 요소의 개수 </br>
\- <a href="https://github.com/cgvvxx/PS/blob/master/ps/DFS%2C%20BFS/101_B_11724.py">[Python]</a>
</details>

<details>
<summary><u><font size="+1"><a href="https://www.acmicpc.net/problem/2468">[boj] 2468 - 안전 영역</a></font></u></summary>

\- 가능한 모든 높이 h에 대하여 dfs를 h 이상일 때만 돌려서 각 h에 대한 영역의 개수를 구하고 그 중 가장 큰 영역의 개수를 출력</br>
\- <a href="https://github.com/cgvvxx/PS/blob/master/boj/boj_2468.py">[Python]</a>
</details>

<details>
<summary><u><font size="+1"><a href="https://www.acmicpc.net/problem/9019">[boj] 9019 - DSLR</a></font></u></summary>
\- 현재의 위치(숫자)에서 다음 경로의 위치(D, S, L, R에 해당하는 명령어의 숫자)를 queue에 넣으면서 bfs 탐색 </br>
\- 이 때 현재까지의 위치(숫자)와 현재까지의 경로(명령어의 나열)을 저장하고, 최종 값이 되는 순간 현재까지의 경로(명령어의 나열)을 출력 </br>
\- <a href="https://github.com/cgvvxx/PS/blob/master/boj/boj_9019.py">[Python]</a>
</details>

<details>
<summary><u><font size="+1"><a href="https://www.acmicpc.net/problem/5427">[boj] 5427 - 불</a></font></u></summary>
\- 불의 위치와 상근이의 위치를 각각 bfs로 탐색해나가면서 상근이가 탈출할 수 있는지 체크 </br>
\- <a href="https://github.com/cgvvxx/PS/blob/master/boj/boj_5427.py">[Python]</a>
</details>
