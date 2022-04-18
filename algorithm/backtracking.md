# Backtracking

백트래킹은 해를 찾는 도중 현재의 상태가 문제의 조건에 맞지 않으면, 현재 상태를 제외하고 다음 단계를 탐색하는 방식이다. 즉, 현재의 노드가 유망하지(promising) 않다고 판단되면 그 노드의 이전으로 돌아가 다음 자식 노드로 가는 가지치기(pruning)를 활용하여 문제를 해결한다. 모든 경우의 수를 전부 고려하는 공간을 트리로 나타낼 수 있을 때 적합한 방식이며, 완전 탐색(brute-force)의 일종이라고 볼 수 있다. 

일반적으로 백트래킹은 트리 구조를 기반으로 DFS로 탐색을 진행하면서 각 노드가 문제의 조건에 맞는지(promising) 체크한 후, 해당 노드가 문제의 조건에 맞지 않으면 dfs 진행을 멈추고(pruning) 이전 노드로 돌아가 다시 dfs 탐색을 진행한다. 따라서 **가지치기를 얼마나 잘하느냐에 따라서 효율성이 결정**된다. 

백트래킹의 대표 예제로 N-Queen 문제가 있다. N-Queen 문제란 NxN 크기의 체스판 위에서 N개의 퀸이 서로 공격할 수 없게 놓는 방법의 수를 구하는 문제이다. 다음 그림은 N=8인 경우, 8-Queen 문제를 해결할 때의 백트래킹 과정을 설명해준다. 

<p align="center"><img src="https://kyun2da.github.io/img/algorithm/nQueen.gif"></p>

<br>

## Pseudo Code

재귀 함수를 활용한 백트래킹의 기본 코드는 다음과 같은 형태이다.

```python
def dfs(cur, ...):
    
    if is_solution(cur): # 종료 조건
        flag = True
        return
    
    for i in range(...): # 현재 상태에서의 모든 경우의 수
        if not is_promising(cur) : # 유망하지(promising) 않은 경우 가지치기(pruning)
            continue
        
        change_state(cur)
        dfs(cur+1, ...)
        rollback_state(cur)
        
        if flag:
            return 
```

- is_solution
  - 현재 노드가 올바른 해인지 체크하는 부분이다. 단순히 현재 노드의 상태가 특정 개수가 되었을 때 종료할 수도 있고, is_solution과 같은 함수를 만들어 현재 노드의 상태가 문제의 조건을 만족하는지 체크하게 할 수도 있다. 검색하는 모든 노드에 대해서 올바른 해인지 체크해야 하므로, 적절한 방법을 이용해서 시간 소요를 최소화 하는 것이 중요하다.
  - 문제의 조건을 만족하는 하나의 solution만 찾아도 된다면 flag를 활용하여 모든 backtracking을 종료 할 수도 있다.
- 반복문
  - 현재 노드에서 나아갈 수 있는 모든 경우의 수를 반복문을 통해서 확인해야 한다. 이 때 가지치기 조건을 반복문에 적용하여 시간 효율성을 높일 수 있다. 
- is_promising
  - 현재 노드가 문제의 조건을 만족하는지 체크하는 부분이다. 마찬가지로 적절한 is_promising 조건을 주어 불필요한 시간 소모를 줄여야 할 수 있다.
- change_state와 rollback_state
  - 현재 노드가 유망하여 dfs를 계속 진행하는 경우 현재의 노드(cur)에서 상태를 변화시키고(change_state), 해당 노드에 대해 dfs 탐색이 끝나면 다시 변화한 상태를 되돌린다(rollback).
  - 또는 dfs 함수의 조건에 현재의 노드(cur)에서 추가한 상태를 parameter로 넘겨주면서 재귀함수를 구성할 수도 있다.

<br>

## 예제

### 1. N과 M(1)

백준 15649 - N과 M(1) 문제에 대한 예제 코드는 다음과 같다. 가장 기본적인 포맷의 백트래킹 예제이다. N과 M(2) ~ (12) 문제들도 비슷한 수준의 예제이니 참고할 것.

```python
def dfs():
    if len(l) == m:
        print(*l)
        return
    
    for i in range(1, n+1):
        if i in l:
            continue
        l.append(i)
        dfs()
        l.pop()


n, m = map(int, input().split())
l = []
dfs()
```

### 2. N-Queen

백준 9663 - N-Qeen 문제에 대한 예제 코드는 다음과 같다. 백트래킹의 가장 대표적인 예제 문제이다. 

```python
def is_diagonal(board, k):
    
    l = board + [k]
    if len(l) <= 1:
        return True
    idx_k = len(l)-1
    
    for i in range(len(l)-1):
        if abs(i - idx_k) == abs(l[i] - k):
             return False
            
    return True

def dfs_chess():
    
    global ans
        
    if len(board) == n:
		ans += 1
        return
    
    for i in range(n):
        if i in board:
            continue
        
        if not is_diagonal(board, i):
            continue
            
        board.append(i)
        dfs_chess()
        board.pop()


n = int(input())
board = []
ans = 0

dfs_chess()
print(ans)
```

### 3. 애너그램

백준 6443 - 애너그램 문제에 대한 예제 코드는 다음과 같다. 

이 때 1의 경우 문자열을 다 만들고 난 후 중복체크를, 2의 경우 문자열을 만드는 과정에서 중복체크를 한다. 이 경우 1은 시간 초과로 문제를 통과하지 못한다. 이와 같이 백트래킹 문제의 경우 결국 완전탐색에서 가지치기를 통해 경우의 수를 줄이는 것이기 때문에 적절한 가지치기를 통해 시간 소요를 줄이는 것이 중요하다. 

```python
# 1.
def dfs(l, visited, n):
    
    if len(l) == n:
        if l not in checked: # 중복체크
            print(l)
            checked.add(l)
        return
    
    for j in range(n):
        if visited & (1 << j):
            continue

        temp = l + words[j]
        dfs(temp, visited | (1 << j), n)
    
for _ in range(int(input())):
    words = sorted(list(input().strip()))
    checked = set()
    dfs('', 0, len(words))
```

```python
# 2.
def dfs(l, visited, n):
    
    if len(l) == n:
        print(l)
        return
    
    for j in range(n):
        if visited & (1 << j):
            continue

        temp = l + words[j]
        if temp not in checked: # 중복체크
            checked.add(temp)
            dfs(temp, visited | (1 << j), n)
    
for _ in range(int(input())):
    words = sorted(list(input().strip()))
    checked = set()
    dfs('', 0, len(words))
```

<br>

## 참고 문제

- [[boj] 15649 - N과 M(1)](https://www.acmicpc.net/problem/15649)
- [[boj] 9663 - N-Queen](https://www.acmicpc.net/problem/9663)

- [[boj] 2580 - 스도쿠](https://www.acmicpc.net/problem/2580)

- [[boj] 6443 - 애너그램](https://www.acmicpc.net/problem/6443)
