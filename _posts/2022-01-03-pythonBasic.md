---
toc: true
title: "[Python] 파이썬 기본 문법 요약"
category:
  - Python
---

## 리스트 list
`list`는 내부적으로 연결리스트 자료구조로 구성되어있다.

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

`import sys`
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
