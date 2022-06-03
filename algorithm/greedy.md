# Greedy

그리디 알고리즘(Greedy Algorithm, 탐욕적 알고리즘)이란, 여러 선택 중 하나의 선택을 해야할 때마다 그 순간에 최적이라고 생각되는 선택을 해나감으로써 최종 해를 구하는 알고리즘이다. 지역적 최적해의 집합이 항상 전역적 최적해가 됨을 보장할 수는 없기 때문에, 그리디 알고리즘을 이용하여 언제나 최적해를 구할 수 있는 것은 아니다. 

예를 들어, 아래와 같이 주어진 트리에서 노드의 합이 최대가 되도록 루트 노드에서 출발하여 리프 노드까지 가는 경로를 찾는다고 하자. 그리디 알고리즘을 이용하면 루트 노드(7)에서 12, 6으로 가는 경로로 이 때의 해는 25이지만, 실제 해는 루트 노드(7)에서 3, 99로 가는 경로로 109가 된다.

<p align="center">
    <img src="https://upload.wikimedia.org/wikipedia/commons/8/8c/Greedy-search-path-example.gif" width="400" height="250">
</p>

<br>

## 그리디 알고리즘의 조건

그리디 알고리즘에서 지역적 최적해가 전역적 최적해가 됨을 보장하려면 다음과 같은 두 가지 조건을 만족해야 한다.

1. **탐욕적 선택 조건(Greedy Choice Property)** : 이전의 선택이 이후의 선택에 영향을 주지 않는다. (또는 모든 경우의 수를 고려하지 않고 그리디한 방식으로 선택하여도 해당 경우에 항상 최적해가 존재한다.)
2. **최적 부분 구조 조건(Optimal Substructure)** : 전체 문제에 대한 최적해를 구하는 방법은 부분 문제에 대한 최적해를 구하는 방법과 일치한다.

따라서 어떠한 문제를 그리디 알고리즘을 이용하여 해결하고자 할 때, 위의 두 가지 조건을 만족하는지 살펴보아야 한다. (다만 해당 문제가 위의 두 가지 조건을 만족하는지 엄밀히 증명하는 것은 쉽지 않을 수도 있다.)  그리디 알고리즘을 활용하는 대표적인 예시로 최소 신장 트리(MST, Minimum Spanning Tree)의 크루스칼 알고리즘(Kruskal Algorithm), 다익스트라 알고리즘(Dijkstra Algorithm) 등이 있다.

일반적으로 그리디 알고리즘의 경우 <u>정렬을 활용하여 해결</u>하는 경우가 많다. 또한 항상 가장 크거나 작은 원소를 찾아야 하는 경우도 존재하므로 <u>우선순위 큐와 같은 자료구조를 활용</u>하기도 한다.

<br>

## 예제

### 1. 분할 가능 배낭 문제(Fractional Knapsack Problem) 

배낭 문제(Knapsack Problem)이란 최대 무게가 정해진 배낭과 각각의 무게와 가치가 주어진 물건들이 주어져있을 때, 배낭에 담은 물건의 가치의 합이 최대가 되도록 하는 문제를 말한다. 이 때 각각의 물건들을 쪼개서 배낭에 담을 수 있는 경우, 분할 가능 배낭 문제(Fractional Knapsack Problem), 물건들을 쪼개서 배낭에 담을 수 없는 경우, 0-1 배낭 문제(0-1 Knapsack Problem)이라고 한다.

분할 가능 배낭 문제의 경우 간단히 그리디 알고리즘을 이용하여 해결할 수 있다. 단순히 무게당 가치가 높은 순서대로 물건들을 정렬한 후, 해당 물건들을 전부 배낭에 담으면 배낭에 담긴 물건의 가치의 합이 최대가 된다.

0-1 배낭 문제는 그리디 알고리즘으로 최적해를 찾을 수 없고, 일반적으로 DP를 활용하여 문제를 해결한다.

### 2. [동전 문제](https://www.acmicpc.net/problem/11047)

가치가 정해져 있는 동전들이 주어져 있고, 동전들의 가치의 합을 특정한 값을 만족하게 하는 동전 개수의 최솟값을 구하는 문제이다. 이 때, 각각의 동전의 가치는 항상 다른 동전의 가치의 배수가 되는 제한 조건이 있다. 

이 문제의 경우도 간단히 그리디 알고리즘을 통해 최적해를 구할 수 있다. 단순히 가장 큰 가치의 동전부터 최대한 많이 사용하면서 해당 값을 만족하도록 하면 동전의 개수가 최소가 된다.

```python
# boj 11047 - 동전 0

N, K = map(int, input().split())
coin_list = []
for i in range(N):
    coin_list.append(int(input()))

coin_list.sort(reverse=True)

count = 0
for coin in coin_list:
    count += K // coin
    K %= coin
    
print(count)
```

다만 동전들의 배수 관계가 없다면 그리디 알고리즘을 통해 해당 문제를 해결할 수 없다. 예를 들어 10원, 70원, 80원 짜리 동전으로 140원을 만든다고 하자. 그리디 알고리즘을 이용하면 80원  1개, 10원 6개로 총 7개의 동전이 해가 되지만, 실제로 70원 2개로 표현하는 경우 동전의 개수가 최소가 되는 최적해이다. 이러한 [문제](https://www.acmicpc.net/problem/2293)는 DP를 활용하여 해결할 수 있다.

### 3. [회의실 배정](https://www.acmicpc.net/problem/1931)

활동 선택 문제(Acitivity Selection Problem) 문제 유형 중 하나인 회의실 예약 문제이다. 시간대가 다른 여러 개의 회의들을 하나의 회의실에서 최대한 많이 진행하고자 할 때, 회의를 최대 몇 개까지 진행할 수 있는지를 구하는 문제이다. 

이 문제의 경우, 그리디 알고리즘을 활용하여 길이와 상관없이 가장 먼저 끝나는 회의부터 선택해 나갈 때 최적해를 구할 수 있다. 위와 같은 방법이 탐욕적 선택 조건과 최적 부분 구조 조건을 만족하는 이유는 다음을 참조하기로 한다. 

- [탐욕스러운 그리디(Greedy) 알고리즘 정리](https://loosie.tistory.com/515)

```python
# boj 1931 - 회의실 배정

import sys
input = sys.stdin.readline

meeting = []
for _ in range(int(input())):
    a, b = map(int, input().split())
    meeting.append((a, b, b-a))
meeting.sort(key=lambda x:(x[1], x[0], x[2]))

ans = 1
start, end = meeting[0][:2]

for meet in meeting[1:]:
    
    if end <= meet[0]:
        ans += 1
        end = meet[1]
        continue
        
    if start >= meet[1]:
        ans += 1
        start = meet[0]
        continue

print(ans)
```

<br>

## 참고문제

<details> 
    <summary>
        <a href="https://www.acmicpc.net/problem/1026">[boj] 1026 - 보물</a>
    </summary> 
    <div markdown="1">
          &nbsp;&nbsp;재배열 부등식 참조 (작은 것은 작은 것끼리, 큰 것은 큰 것끼리 붙여놓을 때 최댓값을, 그 반대일 때 최솟값을 갖는다.) 
    </div> 
</details>

<details> 
    <summary>
        <a href="https://www.acmicpc.net/problem/1715">[boj] 1715 - 카드 정렬하기</a>
    </summary> 
    <div markdown="1">
          &nbsp;&nbsp;우선순위 큐(heap)을 활용하여 각 카드를 우선 순위 큐에 삽입하고 가장 작은 카드부터 2장씩 꺼내서 (pop) 합친 후 우선 순위 큐에 삽입하는 과정을 반복.<br>&nbsp;&nbsp;※ N=1인 경우, 0을 출력해야 함!
    </div> 
</details>

<details> 
    <summary>
        <a href="https://www.acmicpc.net/problem/1700">[boj] 1700 - 멀티탭 스케줄링</a>
    </summary> 
    <div markdown="1">
          &nbsp;&nbsp;그리디하게 현재 사용중인 전기용품 중 가장 마지막에 사용하는 전기용품을 플러그에서 제거하는 방식으로 진행
    </div> 
</details>

