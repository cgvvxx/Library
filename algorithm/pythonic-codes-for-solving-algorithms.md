# Pythonic codes for solving algorithms


## 1. 문자열

```python
import string

print(string.ascii_lowercase)
print(string.ascii_uppercase)
print(string.ascii_letters)
print(string.digits)

>> abcdefghijklmnopqrstuvwxyz
ABCDEFGHIJKLMNOPQRSTUVWXYZ
abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ
0123456789
```

<br>

## 2. 리스트

```python
# 2차원 배열
arr = [[0]*2 for i in range(3)]
print(arr)
>> [[0, 0], [0, 0], [0, 0]

# 2차원 배열 > 1차원 배열
my_list = [[1, 4], [5, 6]]
print(sum(my_list, []))
>> [1, 4, 5, 6]
    
# 행렬의 전치
my_list = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
print(list(zip(*my_list)))
>> [(1, 4, 7), (2, 5, 8), (3, 6, 9)]
    
# 행렬의 회전
my_list = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
print(list(zip(*my_list[::-1])))
>> [(7, 4, 1), (8, 5, 2), (9, 6, 3)]
```

```python
# 1. *(asterisk) ; unpacking
_list = [1, 2, 3, 4, 5]
print(*_list)
>> 1 2 3 4 5

# 2. *(asterisk) ; 가변인자
N, *arr = map(int, input().split())
```

<br>

## 3. 기본 라이브러리

- Python standard library : [collections](https://docs.python.org/ko/3/library/collections.html), [itertools](https://docs.python.org/ko/3/library/itertools.html), [math](https://docs.python.org/ko/3/library/math.html)

### 3.1. collections

#### 3.1.1. Counter

```python
from collections import Counter

_string = 'onetwothree'
Counter(_string)
>> Counter({'o': 2, 'n': 1, 'e': 3, 't': 2, 'w': 1, 'h': 1, 'r': 1})

_list = [1, 2, 3, 1, 1, 4, 2, 5, 4, 4]
Counter(_list)
>> Counter({1: 3, 2: 2, 3: 1, 4: 3, 5: 1})
```

- Counter 객체끼리 +, -, &, | 연산 가능
- most_common(n), update(), elements() etc.

#### 3.1.2. defaultdict

```python
_string = 'hello World'
_string_dict = defaultdict(int)
for char in _string:
    _string_dict[char] += 1
print(_string_dict)
>> defaultdict(int, {'h': 1, 'e': 1, 'l': 3, 'o': 2, ' ': 1, 'W': 1, 'r': 1, 'd': 1}
```

- Dictionary에 값이 있으면 주어진 키로 접근, 값이 없으면 넘겨준 값을 default로 설정

#### 3.1.3. deque

- Stack&Queue 자료구조 참고

### 3.2. itertools

```python
# permutations, combinations, product, combinations_with_replacement
from itertools import ...
_list = [1, 2, 3]

print(*permutations(_list, 2))
print(*combinations(_list, 2))
print(*product(_list, repeat=2)
print(*combinations_with_replacement(_list, 2))

>> (1, 2) (1, 3) (2, 3)
(1, 2) (1, 3) (2, 1) (2, 3) (3, 1) (3, 2)
(1, 1) (1, 2) (1, 3) (2, 2) (2, 3) (3, 3)
(1, 1) (1, 2) (1, 3) (2, 1) (2, 2) (2, 3) (3, 1) (3, 2) (3, 3)
```

### 3.3. math

```python
import math
math.gcd(n, m) # n과 m의 최대공약수
math.lcm(n, m) # n과 m의 최소공배수
math.floor(r) # [r]
```

### 3.4. heapq

- Stack&Queue 참고

### 3.5. bisect

- Binary Search 참고

<br>

## 4. 기타

### 4.1 입출력

```python
import sys
input = sys.stdin.readline
```

- input 보다 더 빠르게 입력을 받을 수 있음 ([참고](https://www.acmicpc.net/blog/view/56))
- sys.stdin.readline의 경우 개행문자(\n) 포함하여 입력으로 받음
- 개행문자(\n)이 있어도 형변환이나 split() 가능

### 4.2 ETC

- 아스키코드 ; ord('char'), chr(num)
- 10진수 > 2, 8, 16진수 ; bin(n), oct(n), hex(n)
- n진수 > 10진수 ; int('n', base)
- dictionary 객체 접근 시 get 사용 ; 해당 key가 존재하지 않으면 None을 리턴

- 객체의 메모리 사이즈 (Byte 단위)

  ```python
  import sys
  
  _list = [i for i in range(2**20)] 
  print("size of list = ", sys.getsizeof(_list)//(1024*1024))
  >> 8 KB
  ```

- 재귀 함수 호출 에러

  - RuntimeError: maximum recursion depth exceeded 발생하는 경우

  ```python
  # default 재귀 함수 호출 횟수 : 1000
  import sys
  
  sys.setrecursionlimit(10**6) 
  ```

