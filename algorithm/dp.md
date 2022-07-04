# DP

동적 계획법(Dynamic Programming)이란 이미 계산된 결과를 메모리에 저장(Memoization)하여 반복하여 계산하지 않도록 하여 시간 효율성 향상시키는 알고리즘 기법이다.

<br>

## 예제 - 피보나치 함수

가장 대표적인 예로, 다음과 같이 재귀함수를 이용한 피보나치 수열의 일반항을 구하는 알고리즘을 작성한다고 하자.

```python
# 일반적인 재귀함수 형태의 알고리즘

def f(n):
    
    call[n] += 1
    
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return f(n-1)+f(n-2)
    
N = 5
call = [0] * (N+1)
print(f"f(5) = {f(N)}")
for i in range(N+1):
    print(f"f[{i}] 호출 횟수 : {call[i]} 번")
    
>> f(5) = 5
f[0] 호출 횟수 : 3 번
f[1] 호출 횟수 : 5 번
f[2] 호출 횟수 : 3 번
f[3] 호출 횟수 : 2 번
f[4] 호출 횟수 : 1 번
f[5] 호출 횟수 : 1 번
```

N=5일 때, f(5)를 구하고자 한다면 다음과 같이 f(1)은 5번, f(2)는 2번과 같이 동일한 함수를 비효율적으로 여러 번 호출하게 된다. 이를 그림으로 살펴보면 다음과 같다. 실제 해당 피보나치 함수를 구하는 알고리즘의 시간복잡도는 &O(N^2)&이다.


<p align="center">
	<img src="https://miro.medium.com/max/617/1*EmGt1nAA3PNd0dcgE24wAw.png">
</p>

여기서 해당 알고리즘을 동적계획법의 메모이제이션(Memoization)을 활용하여 작성하면 다음과 같다. 먼저 첫 번째는 위의 재귀함수로 피보나치 수열을 구하는 함수에 dp 배열에 저장하는 메모이제이션 기법을 추가한 방법이다. 먼저 f(n)을 호출하고 해당 값이 dp[n]에 저장되어 있다면, 하위 함수들을 호출하지 않고 바로 배열의 dp[n] 값을 return, dp[n]에 저장되어 있지 않다면 재귀적으로 f(n-1), f(n-2)의 하위 함수들을 호출하는 과정을 반복하게 된다. 이를 Top-down 방식의 동적계획법 또는 메모이제이션이라고 한다.

```python
# DP ; Top-down 방식

def f(n):
    
    if n==0:
        return 0
    elif n==1:
        return 1
    
    if dp[n] > 0:
        return dp[n]
    
    dp[n] = f(n-1) + f(n-2)
    
    return dp[n]

N = int(input())
dp = [0] * (N+1)
print(f(N))
```

두 번째는 재귀 함수의 형태가 아닌 반복문의 형태를 통해 동적계획법을 활용하는 방법이다. 원하는 피보나치 함수의 값 f(N)을 구하기 위하여 1부터 시작하여 차례대로 구해진 피보나치 함수의 값을 dp 배열에 저장해 놓는다. 이전의 피보나치 함수의 값은 dp 배열에 저장되어 있기 때문에 다음 피보나치 함수의 값을 반복문을 통해 간단히 구할 수 있게 된다. 이를 Bottom-up 방식의 동적계획법 또는 타뷸레이션(Tabulation)이라고 한다.

```py
# DP ; Bottom-up 방식

N = int(input())

dp = [0] * (N+1)
dp[1] = 1

for i in range(2, N+1):
    dp[i] = dp[i-1] + dp[i-2]
    
print(dp[N])
```

피보나치 수열을 구하는 알고리즘을 동적계획법의 메모이제이션을 활용한다면 Top-down 방식이든 Bottom-up 방식이든 시간복잡도는 $O(n)$이 됨을 확인할 수 있다. 실제로 기존의 재귀함수로 작성하는 경우 N=40만 되어도 f(N)을 구하는데 약 30초 가량 소요되지만, 동적계획법을 사용한다면 10ms 내에 해당 값을 구할 수 있다.

<br>

## 동적계획법 vs. 분할 정복

동적계획법(DP)과 분할 정복(Divide and Conquer) 알고리즘 모두 복잡한 문제를 그보다 간단한 여러 개의 문제로 쪼개어 나누어 푸는 방식의 기법을 통해 문제를 해결한다. (특히 동적계획법의 Top-down 방식) 두 알고리즘의 차이점은 동적계획법의 경우 주어진 문제를 잘게 쪼개 작은 문제로 만들었을 때, 작은 문제들이 중복되어 해당 작은 문제들의 해를 기록(Memoization)하여 재활용함으로써 해당 문제의 풀이를 최적화 하는 것이고, 이와 반대로 분할 정복의 경우 부분 문제들이 중복되지 않는다.

예를 들어 위와 같은 피보나치 함수의 경우, 하위로 호출하는 함수들이 중복되어서 호출되므로 동적계획법을 활용한다. 반면 병합 정렬과 같은 경우, 하위 문제들이 각각 독립적이고 중복되지 않으므로 분할 정복 알고리즘을 활용하여 문제를 해결한다.

따라서 일반적으로 다음과 같이 두 가지 조건을 만족하는 경우 동적계획법을 사용한다.

1. 최적 부분 구조(Optimal Substructure) : 큰 문제를 작은 문제로 나누어 푸는 구조
2. 중복 문제 구조(Overlapping Subproblems) : 동일한 작은 문제를 반복적으로 푸는 구조

<br>

## 참고

### 1. 접근방법

동적계획법의 메모이제이션이라는 아이디어는 이해하기는 쉽지만 실제로 문제에 활용하기는 다소 어려워 'Easy to Learn, Hard to Master'의 대표적인 알고리즘이다. 특히 2차원 배열 이상을 활용하여 동적계획법을 사용하는 경우 직관적인 이해가 어려울 때가 많다. 보통 다음과 같이 문제를 접근한다.

1. 해당 문제가 동적계획법을 활용할 수 있는지 먼저 파악해야 한다. 완전탐색을 통해서 문제를 해결할 때 탐색해야할 경우의 수가 얼마나 되는지 생각해본다. 탐색해야할 경우가 너무 많고 그리디와 같은 알고리즘을 사용할 수 없다면 동적계획법을 활용할 수 있는지 고민해본다.
2. N일 때의 값이, 그 이전 수들의 값(예를 들어, 1, 2, ..., N-1까지의 값)을 통해 구할 수 있는지 살펴보고 이를 만족하는 변수들의 관계식(점화식)을 작성한다. 해당 점화식은 1차원 배열에 저장할 수도 있고 2차원 이상의 배열에 저장하는 경우도 있다.
3. 초기 상태(예를 들어, N=0, N=1일 때의 값)을 구한다.
4. Top-down 방식(또는 Memoization)을 활용할지 Bottom-up 방식(또는 Tabulation)을 활용할지 적절한 방법을 선택하여 함수를 구현한다. 일반적으로 Top-down 방식의 경우 직관적으로 해당 알고리즘을 이해하기 쉽다는 장점이 있고 Bottom-up 방식의 경우 반복문을 활용하여 함수를 별도로 호출하지 않으므로 다소의 시간과 메모리를 절약할 수 있다는 장점이 있다.

### 2. 파이썬의 동적계획법

단, 파이썬의 경우 기본 재귀함수 호출 제한이 1000으로 정해져있다. Top-down 방식으로 구현 시 문제의 조건에서 기본 재귀함수 호출 제한 횟수 이상으로 재귀를 하는 경우 다음과 같이 재귀함수 호출 제한 횟수를 정해주어야 한다. 

```python
import sys
sys.setrecursionlimit(10000)
```

파이썬의 경우 재귀함수 호출로 인한 시간 소요가 꽤 발생하므로 파이썬 기준으로는 Top-down 보다는 Bottop-up 방식으로 구현하는 것을 개인적으로 선호한다. 

### 3. 대표 예제

대표적으로 동적계획법을 활용하는 문제들은 다음과 같다.

- 0-1 Knapsack Problem (0-1 배낭 문제)
- LIS Problem (Longest Increasing Subsequence, 가장 긴 증가 수열 문제)
- LCS Problem (Longest Common Subsequence, 최장 공통 부분 수열 문제)
  - **LCS (Longest Common Substring) 문제와 다름**
- Chained Matrix Multiplication (연쇄 행렬 곱셉)
- Bellman-Ford Algorithm (벨먼-포드 알고리즘)
- Floyd-Warshall Algorithm (플로이드-워셜 알고리즘)

#### 3-1. [0-1 Knapsack Problem](https://www.acmicpc.net/problem/12865)

한 여행가가 가지고 있는 배낭에 담을 수 있는 무게의 최댓값이 정해져 있고, 일정 가지와 무게가 있는 짐을 배낭에 넣는다고 할 때 가치의 합이 최대가 되도록 짐을 고르는 문제이다. 짐을 쪼갤 수 있는 경우 Fractional Knapsack Problem이라고 하여 그리디 알고리즘을 활용하여 해결할 수 있다. [(참고)]() 짐을 쪼갤 수 없는 경우를 0-1 Knapsack Problem이라고 하여 이 경우 동적계획법을 활용하여 문제를 해결할 수 있다.

집합 A를 n개의 짐들 중 최적으로 고른 부분집합이라고 하자.

1. 집합 A가 n번째 짐을 포함하고 있지 않다면, A는 n번째 짐을 뺀 나머지 n-1개의 짐들 중에서 최적으로 고른 부분집합과 같다.

2. 집합 A가 n번째 짐을 포함하고 있다면, A에 속한 짐들의 총 가격은 n-1개의 짐들 중에서 최적으로 고른 가치의 합에 n번째 짐의 가치를 더한 것과 같다.

즉, $w_i$를 i번째 짐의 무게, $v_i$를 i번째 짐의 가치, $P[i, w]$를 i번째 짐에 대해 최대 무게가 w일 때의 최적의 가치라고 하면 다음과 같은 점화식을 만족하게 된다.
$$
P[i, w]=\begin{cases}
P[i-1, w] & \mbox{if }w_i > w \\
max\{v_i + P[i-1, w-w_k], P[i-1, w]\} & \mbox{else }
\end{cases}
$$

```python
# boj 12865 - 평범한 배낭

n, k = map(int, input().split())
items = [tuple(map(int, input().split())) for _ in range(n)]

dp = [[0] * (k+1) for _ in range(n)]
for i in range(n):
    for j in range(k+1):
        w, v = items[i]
        
        if i == 0:
            if j >= w:
                dp[i][j] = v
            continue
    
        if j < w:
            dp[i][j] = dp[i-1][j]
        else:
            dp[i][j] = max(dp[i-1][j], dp[i-1][j-w]+v)
                
print(max([dp[i][-1] for i in range(n)]))
```

**Reference**

- [Dynamic Programming: 배낭 채우기 문제](https://gsmesie692.tistory.com/113)

- [0-1 KnapSack Problem and Fractional KnapSack Problem (0-1 배낭 문제, 분할 가능한 배낭 문제) ](https://hi-guten-tag.tistory.com/160)

#### 3-2. LIS Problem

#### 2.2 Longest Increasing Subsequence(LIS, 가장 긴 증가하는 부분 수열)

어떤 임의의 수열이 주어지고 이 수열에서 몇 개의 수들을 제거하여 부분수열을 만든다고 할 때, 만들어진 부분수열 중 오름차순으로 정렬된 가장 긴 수열을 찾는 문제이다. 먼저 기본적인 LIS의 길이를 구하는 방법은 다음과 같다.

- 주어진 배열 A에 대하여 D[i] : A[i]를 마지막 값으로 가지는 가장 긴 증가부분수열의 길이
- D[i] = A[i]가 추가될 수 있는 증가부분수열 중 가장 긴 수열의 길이 + 1
- 이 때 A[0] ~ A[i-1]을 살펴보면서 A[i] 보다 작은 경우 위의 점화식을 업데이트

이러한 방법은 시간복잡도 $O(n^2)$으로 LIS를 구할 수 있다. 이분 탐색을 이용하면 시간복잡도를 $O(nlogn)$ 까지 줄일 수 있다. 이러한 방법은 다음과 같다.

- 위의 방법에서 A[0] ~ A[i-1]의 모든 원소를 살펴보지 않고 증가하는 부분수열의 길이가 k일 때의 수열의 마지막 값의 최솟값을 저장해놓고 이를 통해 D[i]를 구한다

```python
# LIS의 길이 구하기
# 1. 시간 복잡도 ~ O(n^2)
n = int(input())
arr = [0] + list(map(int, input().split()))

dp = [0] * (n+1)
for i in range(1, n+1):
    for j in range(i):
        if arr[j] < arr[i]:
            dp[i] = max(dp[i], dp[j]+1)
            
print(max(dp))
```

```python
# 2. 시간 복잡도 ~ O(nlogn)
from bisect import bisect_left

n = int(input())
arr = [0] + list(map(int, input().split()))

dp = [0] * (n+1)
dp_val = [0]
for i in range(1, n+1):
    
    idx = bisect_left(dp_val, arr[i])
    if idx == len(dp_val):
        dp_val.append(arr[i])
    else:
        dp_val[idx] = arr[i]
    dp[i] = idx
    
print(len(dp_val)-1)
```

<br>

## 예제

### 1. [계단 오르기](https://www.acmicpc.net/problem/2579)

계단은 항상 한 계단 또는 두 계단씩 오르고, 연속된 세 개의 계단은 밟을 수 없는 계단 오르기 문제이다. dp[n]을 n번째 계단을 밟을 때까지 얻을 수 있는 최대 점수값이라고 하자. step[n]이 n번째 계단의 점수라고 하면 $dp[1] = step[1], dp[2] = step[1]+step[2]$ 임은 자명하다.

n번째 계단으로 오는 경우는, n-2번째 계단에서 두 계단씩 오르는 경우 또는 n-1번째 계단에서 한 계단 오르는 경우이다.

1. n-2번째에서 두 계단 오르는 경우는, 그 이전에 한 계단 오르든 두 계단 오르든 상관없이 항상 n번째 계단에 오를 수 있다. 이 경우 $dp[n] = dp[n-2] + step[n]$
2. n-1번째에서 한 계단 오르는 경우는, 연속된 세 개의 계단을 모두 밟을 수 없다는 조건에 의해 n-3번째 계단에서 두 계단 올라서 n-1번째 계단을 밟고 n번째 계단까지 오르는 경우 뿐이다. 이 경우 $dp[n] = dp[n-3] + step[n-1] + step[n]$

따라서 $dp[n] = max(dp[n-3] + step[n-1], dp[n-2]) + step[n]$의 점화식을 만족한다. 이를 이용하여 Bottom-up 방식의 문제 풀이는 다음과 같이 해결할 수 있다.

```python
# boj 2579 - 계단 오르기

n = int(input())
step = [0] + [int(input()) for _ in range(n)]

if n <= 2:
    print(sum(step[:n+1]))
else:
    
    dp = [0] * (n+1)
    dp[1] = step[1]
    dp[2] = step[1] + step[2]

    for j in range(3, n+1):
        dp[j] = max(dp[j-3] + step[j-1], dp[j-2]) + step[j]

    print(dp[n])
```

### 2. [정수 삼각형](https://programmers.co.kr/learn/courses/30/lessons/43105)

정수 적혀있는 삼각형의 꼭대기에서 바닥까지의 경로 중, 숫자의 합이 최대가 되는 경로를 찾는 문제이다. dp\[i\]\[j\]를 (i, j) 까지의 경로 중 숫자의 합의 최댓값이라고 하자. $dp[0][0] = triangle[0][0]$임은 자명하다. i = n인 경우를 생각해보자. 

1. 삼각형의 (i, j)로 오는 경우는 (i-1, j-1)에서 오른쪽으로 오거나, (i-1, j)에서 왼쪽으로 오는 경우 두 가지이다. 따라서 $0 < j < n-1$의 경우에는 $dp[n][j] = max(dp[n-1][j-1], dp[n-1][j]) + triangle[n][j]$이다. 
2. 맨 왼쪽이거나 맨 오른쪽인 경우, 즉 $j=0 이거나 j=n-1$인 경우, $dp[n][j]=triangle[n-1][j] + triangle[n][j]$이다.

이를 이용하여 Bottom-up 방식으로 문제를 해결하면 다음과 같다.

```python
# programmers - 정수 삼각형

def solution(triangle):
    
    count = triangle[:]
    for i in range(len(triangle)):
        for j in range(i+1):
            if i == 0:
                continue
            if j == 0:
                count[i][j] += count[i-1][j]
            elif j == i:
                count[i][j] += count[i-1][j-1]
            else:
                count[i][j] += max(count[i-1][j-1], count[i-1][j])
        
    return max(count[-1])
```

### 3. [스티커](https://www.acmicpc.net/problem/9465)

2차원 배열을 이용한 dp를 통해 문제를 해결할 수 있다. dp를 다음과 같이 정의하자. 
$$
dp[status][c] : 0 \sim c열까지 중 떼어낸 스티커의 점수의 합의 최댓값
$$
이 때, status = 0은 c열에서 위의 스티커를 떼어낸 경우, status = 1은 c열에서 아래의 스티커를 떼어낸 경우, status = 2는 c열에서 스티커를 떼어내지 않은 경우를 의미한다. 따라서 c=0인 경우 (첫번째 열) $dp[0][0] = stickers[0][0], dp[1][0] = stickers[1][0], dp[2][0] = 0$임은 자명하다.

1. status = 0인 경우, 즉 c+1 열에서 위의 스티커를 떼어냈을 때의 점수의 합의 최댓값은 c열에서 아래 스티커를 떼어낸 경우(status=1) 또는 c열에서 스티커를 떼어내지 않은 경우(status=2) 두 가지이다.
2. 마찬가지로 status = 1인 경우는 c열에서 위의 스티커를 떼어낸 경우(status=0) 또는 c열에서 스티커를 떼어내지 않은 경우(status=2) 두 가지이다.
3. status = 2인 경우는 c열의 스티커에 떼어내든 떼어내지 않았든 상관없다.

이를 이용하여 점화식을 구하면 다음과 같다.
$$
dp[0][c+1] = max(dp[1][c], dp[2][c]) + stickers[0][c+1]\\
dp[1][c+1] = max(dp[0][c], dp[2][c]) + stickers[1][c+1]\\
dp[2][c+1] = max(dp[0][c], dp[1][c], dp[2][c])
$$
이를 이용하여 Bottom-up 방식의 문제 풀이는 다음과 같이 해결할 수 있다.

```python
# boj 9465 - 스티커
# Bottom-up 방식

for _ in range(int(input())):
    
    n = int(input())
    stickers = [list(map(int, input().split())) for _ in range(2)]
    
    dp = [[0 for _ in range(n)] for _ in range(3)]
    dp[0][0] = stickers[0][0]
    dp[1][0] = stickers[1][0]
    
    for i in range(n-1):
        
        dp[0][i+1] = max(dp[1][i], dp[2][i]) + stickers[0][i+1]
        dp[1][i+1] = max(dp[0][i], dp[2][i]) + stickers[1][i+1]
        dp[2][i+1] = max(dp[0][i], dp[1][i], dp[2][i])
        
    print(max(dp[0][n-1], dp[1][n-1], dp[2][n-1]))
```

같은 로직을 Top-down 방식으로 구현하면 다음과 같다. set_dp 함수는 Bottom-up 방식의 dp와 같은 값을 return하고, 각 set_dp의 값을 호출할 때 이전 열의 dp 값을 재귀적으로 호출하게 된다.

```python
# boj 9465 - 스티커
# Top-down 방식

import sys
sys.setrecursionlimit(1000000)
# 주어진 문제에서 N <= 100000이므로 그 이상으로 최대 재귀 횟수를 그 이상으로 설정해주어야 한다.

def set_dp(status, c):
    
    if c == 0:
        if status == 2:
            return 0
        else:
            return stickers[status][c]
        
    if dp[status][c] != -1:
        return dp[status][c]
    
    if status == 2:
        dp[status][c] = max(set_dp(0, c-1), set_dp(1, c-1), set_dp(2, c-1))
    else:
        dp[status][c] = max(set_dp(1-status, c-1), set_dp(2, c-1)) + stickers[status][c]
        
    return dp[status][c]


for _ in range(int(input())):
    n = int(input())
    stickers = [list(map(int, input().split())) for _ in range(2)]
    dp = [[-1 for _ in range(n+1)] for _ in range(3)]
    print(max(set_dp(0, n-1), set_dp(1, n-1), set_dp(2, n-1)))
```

<br>

### 참고 문제

<details> 
    <summary>
        <a href="https://www.acmicpc.net/problem/11726">[boj] 11726 - 2*n 타일링</a>
    </summary> 
    <div markdown="1">
          &nbsp;&nbsp;dp[n]은 2*n 크기의 직사각형을 채우는 방법의 수, dp[n+2] = dp[n+1] + dp[n]를 만족 (n+2 크기의 직사각형을 만들려면 n 크기의 직사각형에 =를 추가하거나 n+1 크기의 직사각형에 |를 추가하는 2가지 경우만 존재)
    </div> 
</details>

<details> 
    <summary>
        <a href="https://www.acmicpc.net/problem/11727">[boj] 11727 - 2*n 타일링2</a>
    </summary> 
    <div markdown="1">
          &nbsp;&nbsp;마찬가지로 dp[n]은 2*n 크기의 직사각형을 채우는 방법의 수라고 하면, dp[n+2] = dp[n+1] + 2*dp[n]를 만족 (n+2 크기의 직사각형을 만들려면 n크기의 직사각형에 = 또는 ㅁ을 추가하는 2가지 경우 또는 n+1 크기의 직사각형에 |를 추가하는 1가지 경우가 존재)
    </div> 
</details>

<details> 
    <summary>
        <a href="https://www.acmicpc.net/problem/1463">[boj] 1463 - 1로 만들기</a>
    </summary> 
    <div markdown="1">
          &nbsp;&nbsp;dp[n]을 n을 1로 만드는 최소 횟수라고 하면, dp[n] = min(dp[n/3]+1, dp[n/2]+1, dp[n-1]+1)를 만족 (단, n/3이 정수이거나 n/2가 정수인 경우)
    </div> 
</details>

<details> 
    <summary>
        <a href="https://www.acmicpc.net/problem/10844">[boj] 10844 - 쉬운 계단 수</a>
    </summary> 
    <div markdown="1">
          &nbsp;&nbsp;dp[i][j] = j개의 숫자로 이루어진 수 중 마지막 자리의 수가 i인 계단수의 개수, <br>
        점화식은 dp[i][j] = dp[i-1][j-1] + dp[i+1][j-1] (0 < i < 9), dp[1][j-1] (i==0), dp[8][j-1] (i==9)
    </div> 
</details>

<details> 
    <summary>
        <a href="https://www.acmicpc.net/problem/1932">[boj] 1932 - 정수 삼각형</a>
    </summary> 
    <div markdown="1">
          &nbsp;&nbsp;dp[i][j]는 (i, j) 번째 수로 오는 경로의 합의 최댓값, <br> dp[i][j] = max(dp[i-1][j-1], dp[i-1][j]) + graphs[i][j] (j != 0 and j != i),<br> dp[i][0] = dp[i-1][0] + graphs[i][0], dp[i][i] = dp[i-1][i-1] + graphs[i][i] (j == 0 or j == i)
    </div> 
</details>

<details> 
    <summary>
        <a href="https://www.acmicpc.net/problem/15486">[boj] 15486 - 퇴사 2</a>
    </summary> 
    <div markdown="1">
          &nbsp;&nbsp;dp[i] : i일 전까지(i-1일까지)의 최대 수익라고 하자. i번째 일을 안한다면 dp[i+1] = dp[i], i번째 일을 한다면 dp[i+T[i]] = dp[i] + P[i] <br> 따라서 dp[i+1] = max(dp[i], dp[i+1]), dp[i+T[i]] = max(dp[i+T[i]], dp[i]+P[i])를 만족함
    </div> 
</details>
<details> 
    <summary>
        <a href="https://www.acmicpc.net/problem/14517">[boj] 14517 - 팰린드롬 개수 구하기 (Large)</a>
    </summary> 
    <div markdown="1">
          &nbsp;&nbsp;dp[i][j] : s[i:j]에서의 '부분수열'(연속된 부분 문자열이 아님) 중 팰린드롬의 개수라고 하자. <br>
0. i > j라면 해당 문자열은 존재하지 않으므로 dp[i][j] = 0 <br>
s[i:j]에서의 팰린드롬의 개수는 s[i] != s[j] 라면 <br>
1. i를 포함하지 않는 경우 (s[i+1:j]) 와 <br>
2. j를 포함하지 않는 경우 (s[i:j-1]) 의 개수에 <br>
3-1. i와 j를 모두 포함하지 않는 경우 (s[i+1:j-1]) 의 개수를 빼주어야 한다. (1과 2에서 중복되므로) <br>
단 s[i] = s[j] 라면 <br>
3-2. s[i+1:j-1]에서 s[i]와 s[j]를 양쪽에 추가하면 팰린드롬이 되므로 빼주지 않아도 되고 추가로 s[i], s[j] 자체도 팰린드롬이므로 1을 더해준다. <br>
따라서 다음과 같은 점화식을 만족한다. <br>
dp[i][j] = 0    if i > j <br>
dp[i][j] = dp[i+1][j] + dp[i][j-1] - dp[i+1][j-1]    if s[i] != s[j] <br>
dp[i][j] = dp[i+1][j] + dp[i][j-1] + 1    otherwise
    </div> 
</details>
