# Brute Force

완전 탐색(Brute Force)이란 단순하게 주어진 문제의 가능한 모든 경우의 수를 확인하여 문제의 조건에 맞는 해를 구하는 방법을 말한다.

예를 들어, 4자리로 이루어진 자물쇠의 푼다고 할 때 0000 ~ 9999까지 총 10000가지의 경우의 수를 시도해보면 그 중 한번은 반드시 자물쇠를 풀 수 있는 경우가 존재한다. 이 때 최악의 경우는 10000번째 시도에 해당 자물쇠가 풀리는 경우이다.

완전 탐색 기법은 문제를 접근하는 과정이 직관적이고 항상 확실하게 정확한 해를 구할 수 있지만 가능한 경우의 수가 많을 경우 정해진 시간 내에 답을 내기 어려울 수 있다. 따라서 완전 탐색을 통해 문제를 해결하려고 할 때, 해당 문제의 가능한 경우의 수를 대략적으로 파악하여 문제를 접근하여야 한다. 일반적으로 코딩 테스트 환경에서 파이썬으로 제출한 코드가 1초에 약 2000만 번의 연산을 수행한다고 하고 제한 시간이 1초인 문제를 푸는 경우 데이터의 개수가 100만 개라면 시간 복잡도 O(nlogn) 이내의 알고리즘을 이용하여 해당 문제를 풀어야 한다. 만약 완전 탐색의 시간 복잡도가 O(n^2) 이상이 된다면 해당 문제의 제한 시간 내에 답을 구하지 못하게 된다.

보통 부분 집합 문제나 순열, 조합과 같은 문제들을 완전 탐색으로 푸는 경우 시간복잡도가 O(2^n)또는 O(n!)이므로 n이 충분히 작은 경우에 완전 탐색을 이용하여 문제를 풀어야 한다. (서로 다른 n개의 수를 일렬로 나열하는 순열의 경우의 수는 n!이므로 n=10만 되더라도 전체 경우의 수는 약 400만가지가 되므로 주의하여야 한다)

<br>

## 완전탐색의 기법

일반적으로 다음과 같은 기법들을 완전탐색의 기법이라고 한다.

1. 비트마스크 (집합의 원소의 개수 n개인 경우)
   - 공집합 ; A = 0
   - 전체집합 ; A = (2<<n) - 1
   - k번째 원소 추가 ; A |= (1<<k)
   - k번째 원소 삭제 ; A &= ~(1<<k)
   - k번째 원소 토글(xor) ; A ^= (1<<k)
   - 두 집합의 차집합 ; A & (~B)
2. 재귀함수
3. BFS/DFS
4. 백트래킹
5. 순열

비트마스크의 경우 특정 기법이라고 하기 보다는 비트를 활용하여 시간 효율성이나 공간 효율성을 얻고자 하는 테크닉에 가깝고, 백트래킹 역시 일반적으로 DFS를 통해 구현하며 완전탐색을 하면서 가지치기를 추가하는 방법을 말한다. (해당 내용은 [backtracking](https://github.com/cgvvxx/Library/blob/master/algorithm/backtracking.md) 참조) 순열 또한 DFS(재귀적인) 또는 백트래킹을 통하여 답을 도출하거나 python의 경우 내장 라이브러리(itertools)를 활용할 수 있다.

어떠한 문제를 풀고자 할 때, 완전탐색을 이용하여 문제를 해결하여도 가능한지 여부를 판단하는 것과 실제 완전탐색 문제를 풀면서 적당한 분기처리 혹은 비트마스크를 활용한 테크닉 등을 통하여 제한시간 내에 문제를 해결할 수 있도록 해야한다.

<br>

## 예제

### 1. [사탕 게임](https://www.acmicpc.net/problem/3085)

전체 보드의 크기는 최대 50x50이고, 서로 다른 인접한 두 칸만 바꿀 수 있으므로 최대 경우의 수는 50x49x2개, 약 5000개이다. 따라서 완전탐색을 통해 서로 다른 인접한 두 칸을 바꾼 모든 경우에 대하여, 각 행과 열에서 연결되어 있는 사탕의 개수의 최댓값을 구하여 문제를 해결할 수 있다.

```python
# boj 3085 - 사탕 게임

def count_candy(graphs):     # 해당 보드의 연속된 사탕의 최댓값을 구하는 함수
    
    ans = 1
    for i in range(n):    # 각 행, 열에 대하여 연속된 사탕의 최댓값을 구함
        cnt_r, cnt_c = 1, 1    # cnt_r : 행에서 연속된 사탕의 최댓값, cnt_c : 열에서 연속된 사탕의 최댓값
        for j in range(1, n):
            if graphs[i][j] == graphs[i][j-1]:    # 행에서 이전 사탕과 같으면 cnt += 1
                cnt_r += 1
            else:    # 이전 사탕과 다르면 cnt = 1로 초기화
                cnt_r = 1
                
            if graphs[j][i] == graphs[j-1][i]:    # 열에 대하여 반복
                cnt_c += 1
            else:
                cnt_c = 1
            
            ans = max(ans, cnt_r, cnt_c)
            
    return ans


n = int(input())
graphs = [list(input()) for _ in range(n)]

ans = 1
for i in range(n):
    for j in range(n):
        if i < n-1:    # 열에 대하여 연속된 두 사탕을 바꾸고 count_candy 후에 다시 원복
            graphs[i][j], graphs[i+1][j] = graphs[i+1][j], graphs[i][j]
            ans = max(ans, count_candy(graphs))
            graphs[i][j], graphs[i+1][j] = graphs[i+1][j], graphs[i][j]
        if j < n-1:    # 행에 대하여 반복
            graphs[i][j], graphs[i][j+1] = graphs[i][j+1], graphs[i][j]
            ans = max(ans, count_candy(graphs))
            graphs[i][j], graphs[i][j+1] = graphs[i][j+1], graphs[i][j]
        
print(ans)
```

### 2. [외판원 순회(TSP, Traveling Salesman Problem)](https://www.acmicpc.net/problem/10971)

외판원 순회 문제(TSP, Traveling Salesman Problem)이란 각 도시들끼리 모두 연결되어 있는 n개의 도시를 한 번씩만 방문하여 출발한 도시로 돌아오고자 할 때, 그 경로의 합이 최소가 되는 경로를 찾는 문제를 말한다.

주어진 문제의 경우, n=10으로 총 도시를 방문하는 경우의 수는 10!, 약 400만 가지의 경우만 따지면 되므로 완전탐색으로 해결 가능하다. itertools의 permuations을 이용하여 모든 경우의 수에 대하여 경로의 합을 구하고 그 때의 최솟값을 구하였다.

다만 이 문제의 경우 도시가 연결되어 있지 않는 경우 (graphs\[n][m]=0)가 존재하므로 이 경우 바로 for문을 종료하는 가지치기를 통해 시간초과를 방지해야 한다.

```python
# boj 10971 - 외판원 순회

from itertools import permutations

n = int(input())
graphs = [list(map(int, input().split())) for _ in range(n)]

ans = 10**10
for perm in permutations(range(n)):    # 모든 순열에 대하여 cost를 계산하여 체크
    
    is_possible = True
    check = 0
    
    for i in range(n):
        cost = graphs[perm[i-1]][perm[i]]
        if cost and check < ans:
            check += cost
        else:    # 만약 cost == 0인 경우, 해당 경로로 이동 불가하므로 break
            is_possible = False
            break
            
    if is_possible:
        ans = min(ans, check)
        
print(ans)
```

일반적인 외판원 순회 문제의 경우 DP를 활용하여 해결한다. 다음을 참조

### 3. [링크와 스타트](https://www.acmicpc.net/problem/15661)

최대 20명의 사람을 2개의 조로 나누어 각 조의 능력치의 합(모든 조원 i, j에 대하여 S_ij의 합)을 구한 후 두 조의 능력치의 합의 차이의 최솟값을 구하는 문제이다.

최대 부분집합의 개수는 2^20개, 약 100만 개이므로 완전탐색을 통하여 모든 경우에 대하여 능력치의 합을 구하여 해결할 수 있다. 단순히 itertools의 combinations를 이용하여 풀면 시간 초과가 나와 비트마스크를 사용하였다.

예를 들어, n=4인 경우 1001은 0번째, 3번째 사람이 한 조인 경우, 다른 조는 0110으로 1번째, 2번째 사람이 한 조인 경우로 나타내어 각 조의 능력치의 합을 구한다.

```python
# boj 15661 - 링크와 스타트

def cal_score(b):    # 현재 팀의 능력치를 계산하는 함수
    
    chk = 0
    for i in range(n):
        if (b & (1 << i)):    # i번째 선수가 해당 팀에 존재하면
            for j in range(n):
                if (b & (1 << j)):    # 해당 팀에 존재하는 모든 j에 대하여 능력치를 더함
                    chk += ability[i][j]
                    
    return chk


n = int(input())
ability = [list(map(int, input().split())) for _ in range(n)]

visited = [False] * (1<<n)
ans = 100000

for i in range(1, 1<<n):
    
    j = (1<<n)-i-1
    
    if not visited[i]:
    
        start = cal_score(i)
        link = cal_score(j)

        visited[i] = True
        visited[j] = True

        ans = min(ans, abs(start-link))
        
        if not ans:
            break
        
print(ans)
```

<br>

### 참고 문제

<details>
<summary><u><font size="+1"><a href="https://www.acmicpc.net/problem/10819">[boj] 10819 - 차이를 최대로</a></font></u></summary>

\- 최대 경우의 수는 8!, 약 40000가지 이므로 permutations을 이용하여 완전탐색 </br>
\- <a href="https://github.com/cgvvxx/PS/blob/master/boj/boj_10819.py">[Python]</a>
</details>   

<details>
<summary><u><font size="+1"><a href="https://www.acmicpc.net/problem/15686">[boj] 15686 - 치킨 배달</a></font></u></summary>

\- 치킨 집의 개수 M <= 13이므로 combinations을 통해 완전탐색을 하는 경우 최대 약 2000가지(13C6)의 경우를 살펴보면 충분함 </br>
\- <a href="https://github.com/cgvvxx/PS/blob/master/ps/%EC%99%84%EC%A0%84%ED%83%90%EC%83%89/097_B_15686.py">[Python]</a>
</details>   

<details>
<summary><u><font size="+1"><a href="https://www.acmicpc.net/problem/14500">[boj] 14500 - 테트로미노</a></font></u></summary>

\- dfs와 backtracking을 통하여 4개의 깊이까지 탐색한 후 최댓값을 업데이트 </br>
\- 이 때, ㅗ모양은 dfs를 통해 체크할 수 없으므로 해당 모양의 테트로미노는 직접 체크해주어야함 </br>
\- <a href="https://github.com/cgvvxx/PS/blob/master/boj/boj_14500.py">[Python]</a>
</details>   

<details>
<summary><u><font size="+1"><a href="https://www.acmicpc.net/problem/17142">[boj] 17142 - 연구소 3</a></font></u></summary>

\- bfs + 완전탐색 </br>
\- M개의 바이러스를 선택하는 모든 조합에 대하여 바이러스가 한 칸씩 퍼지는 bfs를 통해 최대 시간을 계산 </br>
\- <a href="https://github.com/cgvvxx/PS/blob/master/boj/boj_17142.py">[Python]</a>
</details>   

<details>
<summary><u><font size="+1"><a href="https://programmers.co.kr/learn/courses/30/lessons/60059">[programmers] 자물쇠와 열쇠</a></font></u></summary>

\- key가 회전하는 경우 4가지 * lock에 key를 대입하는 경우 (N+2M-1)^2가지, 최대 약 15000가지이므로 완전탐색으로 해결 </br>
\- <a href="https://github.com/cgvvxx/PS/blob/master/programmers/programmers_60059.py">[Python]</a>
</details>
