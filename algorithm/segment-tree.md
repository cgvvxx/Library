# Segment Tree

세그먼트 트리(Segment Tree)란 완전 이진 트리 자료구조를 기반으로 주어진 배열에 대한 특정 구간의 정보를 각 노드에 담고 있는 자료구조 또는 그러한 알고리즘을 의미한다.

아래가 주어진 배열의 구간 합 정보를 가지는 세그먼트 트리의 예시이다. 그림과 같이 길이가 16인 배열 [13, -1, 2, 23, -4, 231, 13, 5, 2, -88, -52, 0, 4, 90, 3, -12] 이 주어졌다고 하자. 먼저 루트 노드는 배열의 전체 구간 [0, 15]의 구간 합 정보를 저장한다. 그리고 루트 노드의 자식 노드는 각각 그 절반인 [0, 7] 구간과 [8, 15] 구간의 구간 합 정보를 저장한다. 이를 재귀적으로 그 자식 노드는 또 각각 그 절반인 [0, 3], [4, 7] 구간의 구간 합 정보와 [8, 11], [12, 15] 구간의 구간 합 정보를 저장한다. 이를 반복적으로 적용하면 결과적으로 리프 노드에는 주어진 배열의 각각의 원소의 값을 저장하게 된다. 이렇게 각 노드에 구간 합 정보를 저장하는 세그먼트 트리를 이용하면 단순히 특정 구간의 누적 합 뿐만 아니라, 해당 배열의 특정 값이 변했을 때의 누적 합을 구하는 쿼리가 여러 개 주어질 때 빠른 시간 안에 해결할 수 있다.

<p align="center">
	<img src="https://en.algorithmica.org/hpc/data-structures/img/segtree-path.png">
</p>
주어진 세그먼트 트리의 노드에는 구간 합 뿐만 아니라, 해당하는 구간의 최솟값, 최댓값, 최소값의 위치 등 특정 구간에 해당하는 정보를 저장하여 활용할 수 있다.

<br>

## 세그먼트 트리의 구현

세그먼트 트리를 구현하는 방법으로는 Top-down 방식과 Bottom-up 방식이 존재한다. 위에서 볼 수 있듯이 세그먼트 트리는 재귀적으로 이루어져 있으므로 재귀 함수를 활용하여 Top-down 방식으로 구현할 수도 있고, 반복문을 활용하여 리프 노드부터 채워넣으면서 Bottom-up 방식으로 구현할 수도 있다. 기본적으로 세그먼트 트리를 구현할 때 필요한 함수는 다음과 같다.

1. 배열이 주어졌을 때 세그먼트 트리를 초기화하는 함수 (init)
2. 배열의 값이 바뀌었을 때 세그먼트 트리의 값을 갱신하는 함수 (update)
3. 배열의 주어진 구간에 대한 부분 합을 구하는 함수 (query)

### 1. 재귀적 구조 활용

먼저 세그먼트 트리는 주어진 배열의 길이 * 4 정도의 넉넉하게 설정한다.

```python
tree = [0] * (4*n)    # 세그먼트 트리는 배열의 길이 * 4로 설정
```

먼저 초기화 함수에는 파라미터는 노드의 인덱스(node)와 그 노드에 해당하는 구간의 시작 인덱스(start)와 끝 인덱스(end)를 넘긴다.  (양쪽 인덱스를 포함하는 구간)

1. 시작 인덱스와 끝 인덱스가 같은 경우는 해당 인덱스의 배열 값을 저장하는 리프 노드인 경우이므로 세그먼트 트리에서 해당 노드의 값은 배열의 해당 인덱스의 값과 같다. 
2. 시작 인덱스와 끝 인덱스가 같지 않은 경우는, 주어진 노드의 자식 노드들의 합을 해당 노드에 저장하면 되므로 재귀적으로 자식 노드들의 구간 합을 호출하여 저장하도록 한다.

```python
def init(node, start, end):    # node : 노드의 인덱스, start : 구간의 시작 인덱스, end : 구간의 끝 인덱스
    
    if start == end:    # 리프 노드인 경우, 해당 배열의 값을 저장한다
        tree[node] = arr[start]
        return tree[node]
    else:    # 리프 노드가 아닌 경우, 자식 노드들의 합을 저장한다
        	 # 이 때, 해당 노드의 자식 노드들의 인덱스는 각각 2*node, 2*node+1 이고, 
             # 각 노드들의 해당하는 구간은 start ~ mid, mid+1 ~ end 가 된다. (mid = (start+end)//2)
        mid = (start + end) // 2
        tree[node] = init(2*node, start, mid) + init(2*node+1, mid+1, end)
        return tree[node]

init(1, 0, len(arr)-1)   # 배열이 arr로 주어진 경우, 맨 처음 루트 노드부터 초기화해준다.
						 # 편의상 리프 노드의 인덱스를 1로 한다.
```

다음으로 업데이트 함수에는 파라미터로 노드의 인덱스(node)와 그 노드에 해당하는 구간의 시작 인덱스(start), 끝 인덱스(end), 그리고 배열에서 바꿀 값의 인덱스(idx)와 바꿀 값(value)을 넘긴다.

1. 배열에서 바꿀 값의 인덱스가 현재 노드의 구간 밖에 있는 경우, 해당 노드의 값은 업데이트 할 필요가 없다. 
2. 그렇지 않은 경우에는 먼저 현재 노드의 값에 해당 배열에서 바꿀 값의 변화값(바꿀 값 - 기존 값)을 더해주고, 초기화 함수 때와 마찬가지로 재귀적으로 그 하위 노드들의 값을 업데이트 해준다.

```python
def update(node, start, end, idx, value):    # node : 노드의 인덱스, start : 구간의 시작 인덱스, end : 구간의 끝 인덱스
    										 # idx : 배열에서 바꿀 값의 인덱스, value : 배열에서 바꿀 값
 
    if idx < start or idx > end :    # idx가 [start, end]에 포함되지 않는 경우, 업데이트 하지 않는다.
        return
 
    tree[node] += value - arr[idx]    # 트리에서 현재 노드에서 변화값을 더해준다.
    
    if start != end :    # start != end인 경우, 즉 현재 노드가 리프노드가 아닌 경우 재귀적으로 자식 노드들의 값을 업데이트 해준다.
        mid = (start + end) // 2
        update(2*node, start, mid, idx, value)
        update(2*node+1, mid+1, end, idx, value)
```

다음으로 구간 합을 구하는 쿼리 함수에는 파라미터로 노드의 인덱스(node)와 그 노드에 해당하는 구간의 시작 인덱스(start), 끝 인덱스(end), 그리고 구하고자 하는 구간 합의 왼쪽 인덱스(left)와 오른쪽 인덱스(right)를 넘긴다.

1. 구간 합을 구하고자 하는 구간이 현재 노드의 구간과 겹치지 않는 경우 0을 리턴한다. 
2. 구간 합을 구하고자 하는 구간에 현재 노드의 구간에 온전히 포함되는 경우, 해당 노드의 값을 리턴한다.
3. 그렇지 않은 경우는, 마찬가지로 재귀적으로 자식 노드들을 호출한다.

```python
def query(node, start, end, left, right):    # node : 노드의 인덱스, start : 구간의 시작 인덱스, end : 구간의 끝 인덱스
    										 # left : 구간 합을 구하는 구간의 왼쪽 인덱스, right : 구간 합을 구하는 구간의 오른쪽 인덱스
    
    if left > end or right < start:    # 구간 합을 구하는 구간이 현재 노드의 구간과 겹치지 않는 경우, 0을 리턴한다.
        return 0
    
    if left <= start and end <= right:    # 구간 합을 구하는 구간에 현재 노드의 구간이 온전히 포함되는 경우, 현재 노드의 값을 리턴한다.
        return tree[node]
    
    mid = (start + end) // 2
    
    return query(2*node, start, mid, left, right) + query(2*node+1, mid+1, end, left, right)    # 두 경우에 해당하지 않는 경우는, 재귀적으로 자식 노드들에 대하여 query를 호출한다.
```

따라서 이와 같이 재귀적으로 세그먼트 트리를 구현하는 과정은 다음과 같다.

```python
# 재귀를 이용한 세그먼트 트리

def init(node, start, end):
    
    if start == end:
        tree[node] = arr[start]
        return tree[node]
    else:
        mid = (start + end) // 2
        tree[node] = init(2*node, start, mid) + init(2*node+1, mid+1, end)
        return tree[node]

def update(node, start, end, idx, value):
 
    if idx < start or idx > end :
        return
 
    tree[node] += value - arr[idx]
    
    if start != end :
        mid = (start + end) // 2
        update(2*node, start, mid, idx, value)
        update(2*node+1, mid+1, end, idx, value)

def query(node, start, end, left, right):
    
    if left > end or right < start:
        return 0
    
    if left <= start and end <= right:
        return tree[node]
    
    mid = (start + end) // 2
    
    return query(2*node, start, mid, left, right) + query(2*node+1, mid+1, end, left, right)


arr = [13, -1, 2, 23, -4, 231, 13, 5, 2, -88, -52, 0, 4, 90, 3, -12]
n = len(arr)

tree = [0] * (4*n)
init(1, 0, n-1)

update(1, 0, n-1, 4, 12)    # arr[4] = 12로 업데이트 하는 경우
arr[4] = 12    # 해당 배열 또한 업데이트 시켜주어야 함

print(query(1, 0, n-1, 3, 10))    # arr[3] ~ arr[10]의 구간합을 구하는 경우
```

### 2. 반복문 활용

먼저 세그먼트 트리가 포화이진트리 형태가 되기 위하여 전체 크기를 설정해주어야 한다. 주어진 배열의 길이가 n인 경우 포화이진트리의 길이는 $2\times2^{\lceil log_2 n\rceil}$가 된다. 또한, 세그먼트 트리의 리프 노드의 인덱스는 $2^{\lceil log_2 n\rceil}$ 부터 시작하게 된다.

1. 먼저 세그먼트 트리의 리프 노드부터 주어진 배열의 값을 채운다.
2. 반복문을 통해 리프 노드부터 시작하여 그 부모 노드들의 값을 채워나간다. (마찬가지로 세그먼트 트리의 루트 노드의 인덱스는 편의상 1로 지정한다.)

```python
from math import ceil, log

def init():
    for i in range(size-1, 0, -1):
        tree[i] = tree[2*i] + tree[2*i+1]

n = len(arr)    # 배열이 arr로 주어지는 경우

size = 2**ceil(log(n,2))    # size는 세그먼트 트리에서 리프 노드가 시작하는 인덱스가 되고
tree = [0]*(2*size)    # 세그먼트 트리의 길이는 size*2가 된다.

tree[size:size+n] = arr    # 먼저 세그먼트 트리의 리프 노드들부터 주어진 배열로 채운 후
init()    # 각 리프 노드의 부모 노드들을 반복문을 통해서 채워나간다.
```

다음으로 업데이트 함수의 경우 파라미터로 배열에서 바꿀 값의 인덱스와 바꿀 값을 넘긴다.

1. 먼터 리프 노드부터 바꿀 값을 업데이트 한다.
2. 마찬가지로 반복문을 통해 리프 노드부터 시작하여 부모 노드들의 값을 업데이트 해준다.

```python
def update(idx, value):
    idx += size    # 세그먼트 트리에서 인덱스에 해당하는 노드의 인덱스는 idx + size가 된다.
    tree[idx] = value    # 세그먼트 트리에서 리프 노드부터 바꿀 값으로 업데이트한다.
    while idx > 1:
        idx //= 2    # 리프 노드를 시작으로 그 부모 노드들의 값을 업데이트한다.
        tree[idx] = tree[2*idx] + tree[2*idx+1]
```

마지막으로 주어진 구간의 구간 합을 구하는 쿼리함수는 재귀함수를 통해 구현한 세그먼트 트리에서의 함수와 같은 구조를 갖는다.

따라서 이와 같이 반복문을 활용하여 세그먼트 트리를 구현하는 과정은 다음과 같다.

```python
# 반복문을 이용한 세그먼트 트리

from math import ceil, log


def init():
    for i in range(size-1, 0, -1):
        tree[i] = tree[2*i] + tree[2*i+1]

def update(idx, value):
    idx += size
    tree[idx] = value
    while idx > 1:
        idx //= 2
        tree[idx] = tree[2*idx] + tree[2*idx+1]

def query(node, start, end, left, right):
    
    if left > end or right < start:
        return 0

    if left <= start and end <= right:
        return tree[node]
    
    mid = (start + end) // 2
    
    return query(2*node, start, mid, left, right) + query(2*node+1, mid+1, end, left, right)


arr = [13, -1, 2, 23, -4, 231, 13, 5, 2, -88, -52, 0, 4, 90, 3, -12]
n = len(arr)

size = 2**ceil(log(n,2))
tree = [0]*(2*size)

tree[size:size+n] = arr
init()

update(4, 12)    # arr[4] = 12로 업데이트 하는 경우

print(query(1, 0, n-1, 3, 10))    # arr[3] ~ arr[10]의 구간합을 구하는 경우
```

<br>


