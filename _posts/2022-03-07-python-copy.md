---
toc: true
title: "파이썬 얕은 복사와 깊은 복사"
category:
  - Python
---

## 얕은 복사 $($copy, [:])

```python
arr1 = [1, 2, 3, 4, 5]
arr2 = arr1

print(arr2) # [1, 2, 3, 4, 5]
```

arr1 = [1, 2, 3, 4, 5]이고 arr2 = arr1과 같이 대입연산자를 수행하면 arr2의 출력 결과는 [1, 2, 3, 4, 5]가 됩니다.

하지만 이는 **얕은 복사**로 arr2가 arr1과 같은 메모리 주소를 가리키고 있는 것입니다.

때문에 arr1의 값을 수정, 삽입, 삭제하게되면 arr2에도 변경사항이 동일하게 적용됩니다.

```python
arr1.append(6)

print(arr2) # [1, 2, 3, 4, 5, 6]
```

이와 같이 **참조**만 복사한 것을 얕은 복사라고 합니다.

단, 얕은 복사 개념은 **immutable**한 객체들에는 적용되지 않습니다.

```python
a = 3
b = a # 얕은 복사가 이루어짐. a와 같은 메모리 공간 참조

b = 4 # b는 4가 저장된 새로운 메모리 공간을 참조하게된다.
print(a) # 3
```

immutable 객체들은 **값이 변경되면 무조건 참조가 변경**되기 때문에 얕은 복사를 해서 **b**의 값을 변경하더라도 **a**의 값은 변경되지 않습니다.

이는 immutable 객체들은 값이 변경될 수 없기 때문에 메모리에 새로운 값을 할당해서 객체가 새로운 값의 주소공간을 참조하게 만들기 때문입니다.

따라서 얕은 복사, 깊은 복사에 대한 개념은 값의 변경이 가능한 mutable 객체로 한정됩니다. $($list, set, dictionary)

## mutable 객체를 얕은 복사 하는 방법

### '=' 대입 연산자

**mutable한 객체 $($list)**

```python
arr1 = [1, 2, 3, 4, 5]
arr2 = arr1

print(f"arr1 : {arr1}, {hex(id(arr1))}") # arr1 : [1, 2], 0x8fa0d77b8271
print(f"arr2 : {arr2}, {hex(id(arr2))}") # arr2 : [1, 2], 0x8fa0d77b8271

arr1.append(3)

print(f"arr1 : {arr1}, {hex(id(arr1))}") # arr1 : [1, 2, 3], 0x8fa0d77b8271
print(f"arr2 : {arr2}, {hex(id(arr2))}") # arr2 : [1, 2, 3], 0x8fa0d77b8271
```
arr1의 변경이 arr2에도 영향을 주고 참조하는 주소가 동일하다.

**immutable한 객체 $($int)**

```python
num1 = 10
num2 = num1

print(f"num1 : {num1}, {hex(id(num1))}") # num1 : 10, 0x7fc0d77b8271
print(f"num2 : {num2}, {hex(id(num2))}") # num2 : 10, 0x7fc0d77b8271

num2 += 1
print(f"num1 : {num1}, {hex(id(num1))}") # num1 : 10, 0x7fc0d77b8271
print(f"num2 : {num2}, {hex(id(num2))}") # num2 : 11, 0x7fc0d77k8271
```
값을 변경했을 때 다른 주소를 가리킨다.

### [:] 슬라이싱

**전체 출력**

```python
arr1 = [1, 2, [3, 4]]
arr2 = arr1[:]

print(f'arr1 : {arr1}, {hex(id(arr1))}') # arr1 : [1, 2, [3, 4]], 0x4ff2993
print(f'arr2 : {arr2}, {hex(id(arr2))}') # arr2 : [1, 2, [3, 4]], 0x4ff2e9a
```

arr1과 arr2의 메모리 주소가 다르기 때문에 깊은 복사라고 생각할 수 있습니다.

**리스트 끝에 값 추가**

```python
arr2.append(5)

print(f'arr1 : {arr1}, {hex(id(arr1))}') # arr1 : [1, 2, [3, 4]], 0x4ff2993
print(f'arr2 : {arr2}, {hex(id(arr2))}') # arr2 : [1, 2, [3, 4], 5], 0x4ff2e9a
```

arr2에 값을 추가해 보았더니 arr1에는 값의 변경이 일어나지 않았습니다.

주소값 또한 다르기 때문에 이쯤되면 깊은 복사인게 아닌가 싶습니다.

**리스트 내부 리스트**

```python
print(f'arr1 : {arr1[2]}, {hex(id(arr1[2]))}')# arr1[2] :[3, 4] 0x4ff2993
print(f'arr2 : {arr2[2]}, {hex(id(arr2[2]))}')# arr2[2] :[3, 4] 0x4ff2993
```

리스트 안에 존재하는 내부 리스트인 [3, 4]를 출력해본 결과 같은 주소를 가리키고 있는것을 볼 수 있습니다.

이런 의미에서 얕은 복사라고 할 수 있습니다.

**리스트 내부 리스트에 값 추가**

```python
arr1[2].append(6)

print(f'arr1 : {arr1[2]}, {hex(id(arr1[2]))}')# arr1[2] :[3, 4, 6] 0x4ff2993
print(f'arr2 : {arr2[2]}, {hex(id(arr2[2]))}')# arr2[2] :[3, 4, 6] 0x4ff2993
```

arr1 리스트 내부 리스트에 6을 추가해 보았더니 arr2 내부 리스트에도 6이 추가된 것을 확인할 수 있습니다.

주소값 마저 같으므로 얕은 복사입니다.

**전체 출력**

```python
print(f'arr1 : {arr1}, {hex(id(arr1))}')# arr1 : [1, 2, [3, 4]], 0x4ff2993
print(f'arr2 : {arr2}, {hex(id(arr2))}')# arr2 : [1, 2, [3, 4], 5], 0x4ff2e9a
```

겉은 깊은 복사처럼 보이지만 내부는 얕은 복사로 이루어져 있습니다.

이러한 경우 얕은 복사로 구분합니다.

### copy 메서드 사용
[:]와 동일한 결과가 나옵니다.

```python
arr1 = [1, 2, [3, 4]]
arr2 = arr1.copy()

print(f'arr1 : {arr1}, {hex(id(arr1))}') # arr1 : [1, 2, [3, 4]], 0x4ff2993
print(f'arr2 : {arr2}, {hex(id(arr2))}') # arr2 : [1, 2, [3, 4]], 0x4ff2e9a

arr1[2].append(6)

print(f'arr1 : {arr1[2]}, {hex(id(arr1[2]))}')# arr1[2] :[3, 4, 6] 0x4ff2993
print(f'arr2 : {arr2[2]}, {hex(id(arr2[2]))}')# arr2[2] :[3, 4, 6] 0x4ff2993
```

## 깊은 복사 $($copy.deepcopy)
깊은 복사는 mutable 객체의 내부 객체를 모두 새롭게 만들어서 할당해줍니다.

`copy` 모듈의 **deepcopy** 함수를 사용하여 깊은 복사를 사용할 수 있습니다.

```python
import copy

arr1 = [1, 2, [3, 4]]
arr2 = copy.deepcopy(arr1)

print(f'arr1 : {arr1}, {hex(id(arr1))}') # arr1 : [1, 2, [3, 4]], 0x4ff2993
print(f'arr2 : {arr2}, {hex(id(arr2))}') # arr2 : [1, 2, [3, 4]], 0x4ff2e9a

arr1[2].append(6)

print(f'arr1 : {arr1[2]}, {hex(id(arr1[2]))}')# arr1[2] :[3, 4, 6] 0x4ff2993
print(f'arr2 : {arr2[2]}, {hex(id(arr2[2]))}')# arr2[2] :[3, 4] 0x4ff29edf
```

## 참고 자료
- https://blockdmask.tistory.com/576