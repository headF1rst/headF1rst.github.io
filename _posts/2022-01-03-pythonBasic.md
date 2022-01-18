---
toc: true
title: "[Python] 파이썬 기본 문법 요약"
category:
  - Python
---

## 리스트 list

`list`는 내부적으로 연결리스트 자료구조로 구성되어있다.

C++의 stack을 파이썬에서는 리스트로 대체한다

```python
a = list() # []

n = 8
b = [1] * n # [1, 1, 1, 1, 1, 1, 1, 1]

a.append(1)
a.append(4)
a.insert(1, 3) # a[1]에 값 3을 삽입

print(a) # [1, 3, 4]
print(a[-1]) # 4 (뒤에서 첫번째 원소 출력)
print(a[1:3]) # [3, 4] (1에서 3번째 원소 까지 출력)

a.reverse()
print(a) # [4, 3, 1]

a.remove(4)
print(a) # [3, 1]

a.sort()
print(a) # [1, 3]
a.sort(reverse=True)
print(a) # [3, 1]

print(a.count(3)) # 1
```

### 리스트 컴프리헨션

특정 크기의 2차원 리스트를 초기화할 때 사용.

```python
n = 3
m = 4

# n x m 크기의 2차원 배열 0으로 초기화
a = [[0] * m for _ in range(n)]
print(a) # [[0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0]]

# 1 ~ 9를 포함하는 리스트
a = [i * i for i in range(1,10)]
print(a) # [1, 4, 9, 16, 25, 36, 49, 64, 81]
```

### 특정한 원소값 모두 제거

`remove_set`에 포함된 원소들을 모두 제거

```python
arr = [1, 1, 1, 2, 3, 4, 5]
remove_set = {1, 4}

res = [i for i in arr if i not in remove_set]

# for i in arr :
#     if i not in remove_set:
#         res.append(i)

print(res) # [2, 3, 5]
```

### 리스트 메서드 시간복잡도

| 메서드             | 시간복잡도 |
| ------------------ | ---------- |
| copy               | `O(n)`     |
| append             | `O(1)`     |
| pop                | `O(1)`     |
| insert             | `O(n)`     |
| getitem            | `O(1)`     |
| setitem            | `O(1)`     |
| delitem            | `O(n)`     |
| sort               | `O(nlogn)` |
| x in s             | `O(n)`     |
| min$($s), max$($s) | `O(n)`     |

## 문자열

문자열은 내부적으로 리스트와 같다.

```python
a = "Hello"
print(a * 3) # HelloHello

print(a[2:4]) # ll
```

## 튜플 tuple

- 한번 초기화된 값은 변경 불가.
- 소괄호 `()`를 사용하여 값을 순차적으로 저장.
  - `a = (1, 2 ,3)`
- 리스트에 비해 공간 효울적이다.
- 서로다른 성질의 데이터를 묶어서 저장하는데 주로 사용됨.

## 사전 자료형 dict$($)

- 데이터를 **key - value** 형태로 저장.
- 내부적으로 해시 테이블을 이용.
- 데이터의 검색이 빠름. `O(1)`

**관련 함수**

**`keys()` :** 키 데이터만 모은 리스트 생성

**`values()` :** 값 데이터만 모은 리스트 생성

```python
data = dict()
data['수학'] = 'Math'
data['음악'] = 'Music'
data['과학'] = 'Science'

key_list = data.keys()
print(key_list)

data.pop('수학')

key_values = data.values()
print(key_values) # ['Music', 'Science']

data.clear()

if '과학' in data : # 과학이 존재하지 않음.
    print("과학이 존재합니다.")
```

## 집합 자료형 set$($)

- 중복을 허용하지 않는다.
- 순서가 없다. `인덱싱을 통해 값을 찾지 못한다.`
- 특정 데이터가 등장한 적 있는지 여부판단에 유용.

### 연산

- **합집합 :** `|`
- **교집합 :** `&`
- **차집합 :** `-`

**`update()` :** 여러개의 값을 한번에 추가

```python
a = set([1, 2, 2, 2, 4, 5]) # {1, 2, 4, 5}
b = {1, 1, 3, 5, 6} # {1, 3, 5, 6}

print(a | b) # set([1, 2, 3, 4, 5, 6])
print(a & b) # set([1, 5])
print(a - b) # set([2, 4])

a.add(9) # O(1)
print(a) # set([1, 2, 4, 5, 9])

b.update([10, 11])
print(b) # set([1, 3, 5, 6, 10, 11])

a.remove(9) # O(1)
print(a) # set([1, 2, 4, 5])
```

## 파이썬의 연산자

여러 데이터를 저장하고 있는 자료형에 특정 값의 존재 여부를 확인하는 연산자로 `in`과 `not in` 연산자를 제공한다.

- **`X in 리스트` :** 리스트에 `X`가 존재하면 True.
- **`X not in 문자열` :** 문자열에 `X`가 존재하지 않으면 True.

## 전역변수 global

함수에서 지역변수로 생성하지 않고 밖에 선언된 변수를 참조.

```python
sum = 0

def func(num) :
    global sum # sum 전역변수 선언
    sum += num;

for i in range(11) : # 1 ~ 10의 합
    func(i)

print(sum) # 55
```

## 입출력

`input()` : 한줄의 문자열을 입력받는다.

**한 줄에 하나의 데이터를 입력받는 경우 :**

`int(intput())`

**데이터가 한줄에 공백으로 구분되어 입력되는 경우 :**

`list(map(int, input().split()))`

- `input()`으로 문자열 입력받음.
- `.split()` 통해 공백을 기준으로 문자열 파싱
- `map()`을 사용하여 파싱된 리스트의 모든 원소에 `int()` 함수 적용.
- `list()`로 리스트 형태로 저장.

**더 빠른 입력방법 :**

`import sys` <br>
`sys.stdin.readline().rstrip()`

**출력**

`f-string` 문법을 통해 `{}`안에 변수를 넣어서 형변환 없이 문자열과 정수를 같이 출력할 수 있다.

```python
import sys
num = int(input()) # 데이터 하나 입력

data = list(map(int, input().split())) # 공백으로 구분하여 입력
n, m, k = map(int, input().split())

fdata = sys.stdin.readline().rstrip()

print(f"num은 {num}입니다.") # num은 2입니다.
```

## 내장함수

별도의 `import`없이 사용할 수 있다.

**`sum()` -** `iterable`객체의 모든 원소 합을 반환.

```python
result = sum([1,2,3,4,5])
print(result) # 5
```

**`min()` & `max()` -** 가장 작은값, 가장 큰 값을 반환.

```python
Min = min([1,2,3,4,5])
Max = max([1,2,3,4,5])

print(Max) # 5
print(Min) # 1
```

**`eval()` -** 문자열 형식의 수학 수식을 계산하여 반환.

```python
result = eval("(3+8) / 2")
print(result) # 5
```

**`sorted()` -** `iterable`객체의 원소들을 오름차순 정렬.

key 속성으로 정렬기준을 명세할 수 있다.

ex) `key = lambda x : x[1]`

```python
# 두번째 원소를 기준으로 내림차순
result = sorted([('Harry', 90), ('Jadon', 75), ('Phil', 95)], key = lambda x : x[1], reverse=True)
print(result) # [('Phil', 95), ('Harry', 90), ('Jadon', 75)]
```

## itertools

반복되는 데이터를 처리하는데 유용한 라이브러리.

---

**`permutations`**

iterable 객체에서 r개의 데이터를 뽑아 일려로 나열하는 모든 경우를 계산.

```python
from itertools import permutations

data = ['a', 'b', 'c']
result = list(permutations(data, 3)) # 모든 순열 구하기
print(result)
```

---

**`cobinations`**

iterable객체에서 r개의 데이터를 뽑아 순서를 고려하지 않고 나열하는 모든 경우를 계산.

```python
from itertools import combinations

data = ['a', 'b', 'c']
result = list(combinations(data, 3))
result2 = list(combinations(data, 2)) # 2개를 뽑는 모든 조합 구하기
print(result) # [('a', 'b', 'c')]
print(result2) # [('a', 'b'), ('a', 'c'), ('b', 'c')]
```

---

**`product`**

iterable 객체에서 r개의 데이터를 뽑아 **일렬로 나열**하는 모든 경우를 계산.

단, 원소를 **중복하여** 뽑는다.

뽑고자 하는 데이터의 수는 `repeat` 속성값으로 할당.

```python
from itertools import product

data = ['a', 'b', 'c']
result = list(product(data, repeat=2)) # 2개를 뽑는 모든 수열 구하기(중복 허용)
print(result) # [('a', 'a'), ('a', 'b'), ('a', 'c'), ('b', 'a'), ('b', 'b'), ('b', 'c'), ('c', 'a'), ('c', 'b'), ('c', 'c')]
```

---

**`combinations_with_replacement`**

iterable객체에서 r개의 데이터를 뽑아 **순서를 고려하지 않고** 나열하는 모든 경우를 계산하며 원소를 **중복해서** 뽑는다.

```python
from itertools import combinations_with_replacement

data = ['a', 'b', 'c']
result = list(combinations_with_replacement(data, 2)) # 2개를 뽑는 모든 조합 구하기(중복 허용)
print(result) # [('a', 'a'), ('a', 'b'), ('a', 'c'), ('b', 'b'), ('b', 'c'), ('c', 'c')]
```

## heapq

`힙(Heap)` 기능을 제공하며 힙은 최소 힙으로 구성되어있다.

최소 힙 자료구조의 최상단 원소는 가장 작은 원소이며 원소를 힙에 넣었다가 빼는 과정만으로도 원소의 오름차순 정렬이 가능하다. `O(NlogN)`

C++의 priority_queue 대신 파이썬에선 `heapq`를 사용.

**`heapq.heappush(리스트, 원소값)` :** 힙에 원소 삽입

**`heapq.heappop(리스트)` :** 힙에서 원소 추출

**`heapq.heapify(리스트)` :** 리스트를 선형 시간으로 제자리에서 힙으로 변환.

**오름차순 정렬**

```python
import heapq

# heapq로 힙 정렬 (오름차순)
def heapSort(iterable) :
    h = [] # 힙으로 사용될 리스트
    result = []

    # 모든 원소를 차례대로 힙에 삽입
    for value in iterable:
        heapq.heappush(h, value)

    for i in range(len(h)):
        result.append(heapq.heappop(h))

    return result

ans = heapSort([1, 4, 5, 9, 2])
print(ans) # [1, 2, 4, 5, 9]
```

**내림차순 정렬**
최대 힙을 제공하지 않으므로 힙에 원소를 삽입할 때 부호를 바꿨다가 뺄때 다시 부호를 바꾸는 식으로 구현.

```python
import heapq

# heapq로 힙 정렬(내림차순)
def heapSort(iterable):
    h = []
    result = []

    for value in iterable:
        heapq.heappush(h, -value)

    for i in range(len(h)):
        result.append(-(heapq.heappop(h)))

    return result

ans = heapSort([2, 4, 1, 3])
print(ans) # [4, 3, 2, 1]
```

## bisect

이진탐색 구현을 위한 라이브러리.

정렬된 배열에서 특정 원소를 찾는데 효과적이다.

**`bisect_left(a, x)` :** 정렬된 순서를 유지하면서 리스트 `a`에 데이터 `x`를 삽입할 가장 왼쪽 인덱스를 찾는다. `O(logN)`

**`bisect_right(a, x)` :** 정렬된 순서를 유지하면서 리스트 `a`에 데이터 `x`를 삽입할 가장 오른쪽 인덱스를 찾는다. `O(logN)`

```python
from bisect import bisect_left, bisect_right

a = [1, 2, 4, 4, 8]
print(bisect_left(a, 4)) # 2
print(bisect_right(a, 4)) # 4
```

### 정렬된 리스트에서 특정 범위에 속하는 원소의 개수 구하기

bisect을 사용해서 **right index - left index**를 통해

`left_value <= x <= right_value`인 원소의 개수를 `O(logN)`으로 개산 가능.

```python
# a 리스트 내에 4 ~ 9까지 원소의 수를 구하는 예제
from bisect import bisect_right, bisect_left

a = [2, 4, 4, 5, 6, 9, 10]

left_idx = bisect_left(a, 4) # 1
right_idx = bisect_right(a, 9) # 6

cnt = right_idx - left_idx
print(cnt) # 5
```

## collections

`dequeue`, `Counter` 자료구조 클래스등을 제공하는 라이브러리.

### deque$($)

파이썬에서 `deque`를 queue로 사용. $($스택의 대용도 가능.)

**`popleft()` -** 첫번째 원소를 제거 `O(1)`

**`pop()` -** 마지막 원소를 제거 `O(1)`

**`appendleft(x)` -** 첫번째 인덱스에 원소 x 삽입 `O(1)`

**`append(x)` -** 마지막 인덱스에 원소 x 삽입 `O(1)`

```python
from collections import deque

data = deque([1, 2, 3])

data.appendleft(9) # [9, 1, 2, 3]
data.append(10) # [9, 1, 2, 3, 10]

print(data[0]) # 9

front = data.popleft() # [1, 2, 3, 10]
print(front) # 9
data.pop() # [1, 2, 3]

print(data) # deque([1, 2, 3])
```

### Counter$($)

iterable객체 내부의 원소가 몇번 등장했는지 알려주는 기능.

원소별 등장 횟수를 세는데 활용.

iterable객체는 `Counter`에 의해 사전형 key-value 형태로 저장됨.

```python
from collections import Counter

a = ['red', 'red', 'red', 'blue', 'green', 'green']

counter = Counter(a)

print(counter) # Counter({'red': 3, 'green': 2, 'blue': 1})
print(counter['red']) # 3
print(counter['blue']) # 1
print(dict(counter)) # {'blue': 1, 'green': 2, 'red': 3}
```

counter에는 value의 내림차순으로 정렬이 되어있다.

사전형으로 변환하면 value의 오름차순으로 정렬이 되어있다.

## math

```python
import math

# 5! 출력
print(math.factorial(5)) # 120

# 7의 제곱근 출력
print(math.sqrt(7)) # 2.64575131106

# 21과 14의 최대 공약수 출력
print(math.gcd(21, 14)) # 7

print(math.pi) # 3.14159265359
print(math.e) # 2.71828182846
print(abs(-1)) # 1
```
