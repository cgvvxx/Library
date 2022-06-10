# Divide and Conquer

분할 정복(Divide and Conquer)이란 주어진 문제를 더 작은 단위의 하위 문제로 분할하고, 하위 문제를 해결한 후 다시 이를 조합하여 주어진 문제의 해를 찾는 방법을 말한다.

아래 그림이 대표적인 분할 정복 알고리즘을 적용하여 문제를 해결하는 방법이다. 주어진 문제를 비슷한 크기의 두 개의 부분 문제로 분할하고, 또 그 부분 문제를 비슷한 크기의 두 개의 부분 문제로 분할하는 과정을 진행한다. 이 때 해당 부분 문제가 충분히 간단한 경우(일반적으로 전체 경우의 수가 한 가지 또는 두 가지가 되는 경우) 해당 부분 문제를 해결한다. 각 부분 문제의 해를 return 하여 그 상위 문제의 해를 하위 부분 문제들의 조합으로 구한다. 이러한 과정을 원래의 문제의 해를 구할 때까지 반복하여 해결한다.

<p align="center">
	<img src="https://w.namu.la/s/739414961f779b823a01f6ee005d0b88b4c360bc903b2a6c1d8cc066766f939d322d860e204f587cff859e02a70941b07ba50f66ed5c39efba6d3071db2aa6f14e978e6f13e1efc9cedfb45b5c05aa38a160bba01d2a7fb5004a961ec0191f4a3b6dd5cb73dffac2737bf0b0c8f4607e">
</p>


<br>

## Pseudo Code

일반적으로 재귀 함수를 이용하여 분할 정복의 의사 코드는 다음과 같은 형태이다. 

```python
def dq(x):
    
    if is_small(x):    # x가 충분히 작은 하위 문제로 쪼개진 경우
        return conquer(x)    # 문제의 조건에 맞는 해를 return
    else:
        x1, x2 = divide(x)    # 아닌 경우, x를 더 작은 x1, x2로 쪼개어
        y1, y2 = dq(x1), dq(x2)    # f(x1), f(x2)를 구하여
        y = combine(y1, y2)    # 두 해를 합쳐서 return
        return y
```

- 일반적으로 분할(Divide) > 정복(Conquer) > 조합(Combine)의 과정으로 분할 정복 문제를 해결한다.
- 분할 : divide
  - x의 크기가 큰 경우, 더 작은 두 개의 부분 문제 x1, x2로 나눈다.
  - 이 때, 각각의 부분 문제 x1, x2의 해는 재귀적으로 dq(x1), dq(x2)를 호출하여 해결한다. (다만, 재귀적으로 문제를 해결하는 과정에서 시간적으로나 공간적으로 비효율적일수도 있다.)

- 정복 : is_small & conquer
  - 현재 x의 상태가 충분히 작을 때(혹은 충분히 계산 가능한 부분 문제의 형태일 때)의 이 때의 해를 구하여 return 한다. 이 때, 해당 부분 문제가 어느 정도로 간단한 지 결정하는 is_small 함수의 형태에 따라 알고리즘의 성능이 차이가 날 수도 있다. 

- 조합 : combine
  - 분할하여 얻은 두 개의 부분문제 x1, x2의 해를 이용하여 원래 문제 x의 해를 구하여 return 한다.
  - 서로 다른 두 부분문제 x1, x2의 조합을 하는 경우, x1과 x2에 걸쳐져 있는 해를 추가로 고려하여야 하는 경우도 존재한다. (ex. 히스토그램에서 가장 큰 직사각형)

<br>

## 시간복잡도

일반적으로 많은 알고리즘은 다음과 같은 수식을 만족한다.

$$T(n) = aT(n/b) + f(n)$$

이 때, $a >= 1,  b>1$이고, $f(n)$은 점근적으로 양수 함수값을 가지는 함수이다. 이러한 형식의 알고리즘은 마스터 정리에 의하여 다음과 같은 시간복잡도를 가진다.

1. $f(n) = O(n^c), c < log_b a$이면, $T(n) = {\Theta}(n^{{log_b}a})$
2. $f(n) = {\Theta}(n^c), c = log_b a$이면, $T(n) = {\Theta}(n^c log n))$
3. $f(n) = {\Omega}(n^c), c > log_b a$이면, $T(n) = {\Theta}(f(n))$

예를 들어서, n개의 원소를 정렬하는 병합 정렬의 경우를 살펴보자. 각각의 크기가 절반인 두 개의 하위 문제로 나누어 풀고난 후, 다시 두 개의 하위 문제로 나온 n개의 원소들을 정렬해야 하므로 $T(n)=2T(n/2)+O(n)$을 만족한다. 따라서 위의 정리에 의하여 병합 정렬의 시간복잡도는 O(nlogn)이 된다.

<p align="center">
	<img src="https://s3.ap-south-1.amazonaws.com/afteracademy-server-uploads/merge-sort-recursion-tree-43fd97fbb9647c1a.png">
</p>

**Reference**

- [Analysis of Algorithm (Solving Recurrences) - GeeksforGeeks](https://www.geeksforgeeks.org/analysis-algorithm-set-4-master-method-solving-recurrences/)
- [Merge Sort - afteracademy](https://afteracademy.com/blog/merge-sort)

## 예제

대표적으로 [퀵 정렬](https://github.com/cgvvxx/Library/blob/master/algorithm/sorting.md)이나 [병합 정렬](https://github.com/cgvvxx/Library/blob/master/algorithm/sorting.md), 혹은 [이진 탐색]()의 경우 분할 정복 알고리즘을 활용하여 문제를 해결한다.

### 1. [곱셈](https://www.acmicpc.net/problem/1629)

일반적으로 n차의 거듭제곱을 구할 때의 시간복잡도는 O(n)이지만 분할 정복 알고리즘을 활용하면 n차의 거듭제곱을 O(logn)의 시간복잡도로 구할 수 있다.

해당 문제는 $a^b$를 c로 나눈 나머지를 구하는 문제이다. a, b, c 모두 약 20억까지의 크기를 가지므로 단순히 n번의 곱셈으로 문제를 해결하려고 하면 시간초과가 난다. 분할 정복을 이용하면 다음과 같이 해결할 수 있다.

1. (분할) d = b//2라고 할 때, b가 짝수이면 b를 d, d로 b가 홀수이면 d, d, 1로 나눈다
2. (정복) b == 1이 될때까지 나누어 이 경우 a%c를 리턴한다.
3. (병합) b가 짝수이면 $a^b$를 $a^d{\times}a^d$의 값을, b가 홀수이면 $a^d{\times}a^d{\times}a$의 값을 계산하여 리턴한다.

```py
# boj 1629 - 곱셈

def pow(a, b, c):
        
    if b == 1:    # b == 1인 경우, a를 c로 나눈 나머지를 리턴
        return a % c
    else:
        x = pow(a, b//2, c)    # b가 짝수이면 x*x로, b가 홀수이면 x*x*a로 분할
        if b % 2 == 0:
            return (x * x) % c
        else:
            return (x * x * a) % c

print(pow(*map(int, input().split())))
```

비슷한 예제로 [행렬의 거듭제곱](https://www.acmicpc.net/problem/10830)의 경우에도 분할 정복 알고리즘을 이용하여 해결할 수 있다. [피보나치 수열](https://www.acmicpc.net/problem/11444)의 경우 또한 마찬가지 방법을 통해서 해결할 수 있다.

### 2. [색종이 만들기](https://www.acmicpc.net/problem/2630)

한 변의 길이가 n($2^k$ 꼴)인 색종이를 4등분 해나가면서 전체 색종이가 같은 색깔인 경우, 해당 색깔의 갯수를 세는 문제이다. 마찬가지로 다음과 같은 과정으로 분할 정복 알고리즘을 이용하여 문제를 해결할 수 있다.

1. (분할) m = 2//n으로 나누어 주어진 색종이(2차원 배열)을 4개의 색종이로 나눈다
2. (정복) 주어진 색종이의 2차원 배열이 하나의 색으로만 이루어질 때, 그 색의 변수를 1을 더한다.

```python
# boj 2630 - 색종이 만들기

def get(arr, x1, x2, y1, y2):    # 현재 배열의 좌상단(x1, y1)과 우하단(x2, y2) 영역의 배열을 리턴
    
    return [[arr[x][y] for y in range(y1, y2)] for x in range(x1, x2)]


def dq(arr):
    
    global white, blue
    
    chk = set(sum(arr, []))
    
    if len(chk) == 1:    # 현재 색종이가 하나의 색으로 칠해진 경우, 그 색에 1을 더함
        if chk == {1}:
            blue += 1
        else:
            white += 1
    else:    # 아닌 경우, 4등분하여 각각의 좌표들로 dq를 재귀적으로 호출
        n = len(arr)
        m = n // 2
        dq(get(arr, 0, m, 0, m))
        dq(get(arr, 0, m, m, n))
        dq(get(arr, m, n, 0, m))
        dq(get(arr, m, n, m, n))


n = int(input())
papers = [list(map(int, input().split())) for _ in range(n)]

white, blue = 0, 0
dq(papers)

print(white, blue, sep='\n')
```

### 3. [히스토그램](https://www.acmicpc.net/problem/1725)

히스토그램 내부에서 가장 큰 직사각형의 넓이를 구하는 문제이다. 이 때, 분할 정복 알고리즘을 이용하여 다음과 같은 직사각형들의 넓이의 최댓값을 구하면 된다.

1. 분할점(가운데)를 기준으로 왼쪽에서 가장 큰 직사각형의 넓이
2. 분할점(가운데)를 기준으로 오른쪽에서 가장 큰 직사각형의 넓이
3. 분할점을 포함하는 가장 큰 직사각형의 넓이

이 때, 3의 분할점을 포함하는 직사각형의 넓이를 구하는 과정을 잘 구현해야 한다. 해당 문제에 대한 자세한 설명은 다음을 참조한다.

- [히스토그램에서 가장 큰 직사각형](https://st-lab.tistory.com/255)

```python
# boj 1725 - 히스토그램

def dq(s, e):
    
    if s == e:
        return 0
    elif s+1 == e:
        return hist[s]
    
    mid = (s + e) // 2
    area = max(dq(s, mid), dq(mid, e))    # mid를 기준으로 왼쪽과 오른쪽 직사각형의 최댓값
    
    l, r = mid, mid
    w, h = 1, hist[mid]
    
    while r-l+1 < e-s:
        
        p = min(h, hist[l-1]) if l > s else -1
        q = min(h, hist[r+1]) if r < e-1 else -1
        
        if p >= q:
            h = p
            l -= 1
        else:
            h = q
            r += 1
        
        w += 1
        area = max(area, w*h)
    
    return area


n = int(input())
hist = [int(input()) for _ in range(n)]
print(dq(0, n))
```

비슷한 문제로 좌표평면 상에 주어진 n개의 점들 중, 가장 가까운 두 점을 구하는 문제인 [Closest Pair of Points](https://www.acmicpc.net/problem/2261) 문제가 있다.

<br>

### 참고 문제

<details> 
    <summary>
        <a href="https://www.acmicpc.net/problem/1992">[boj] 1992 - 쿼드트리</a>
    </summary> 
    <div markdown="1">
          &nbsp;&nbsp;주어진 영상을 4개로 쪼개 각각의 영상이 하나의 문자로 이루어진 경우, 바로 그 문자값을 리턴하고, 4개로 쪼개지는 경우는 앞 뒤로 "("와 ")"을 붙여서 리턴
    </div> 
</details>

<details> 
    <summary>
        <a href="https://www.acmicpc.net/problem/2447">[boj] 2447 - 별 찍기 10</a>
    </summary> 
    <div markdown="1">
          &nbsp;&nbsp;먼저 주어진 길이만큼의 리스트를 만들고, 재귀적으로 3등분한 내부 구역에 대하여 별 찍기
    </div> 
</details>
<details> 
    <summary>
        <a href="https://www.acmicpc.net/problem/2104">[boj] 2104 - 부분배열 고르기</a>
    </summary> 
    <div markdown="1">
          &nbsp;&nbsp;히스토그램 내의 최대 직사각형의 크기를 구하는 문제와 비슷한 방식의 분할 정복 알고리즘을 활용. 이 때, 가운데를 포함한 수열에 대해서 가로의 길이가 수열의 합, 세로의 길이가 각 수열의 값들의 최솟값으로 진행
    </div> 
</details>

