---
toc: true
title: "[Python] ���̽� �⺻ ���� ���"
category:
  - Python
---

## ����Ʈ list
`list`�� ���������� ���Ḯ��Ʈ �ڷᱸ���� �����Ǿ��ִ�.

```python
a = list() # []

n = 8
b = [1] * n # [1, 1, 1, 1, 1, 1, 1, 1]

a.append(1)
a.append(4)
a.insert(1, 3) # a[1]�� �� 3�� ����

print(a) # [1, 3, 4]
print(a[-1]) # 4 (�ڿ��� ù��° ���� ���)
print(a[1:3]) # [3, 4] (1���� 3��° ���� ���� ���)

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

### ����Ʈ ���������

Ư�� ũ���� 2���� ����Ʈ�� �ʱ�ȭ�� �� ���.

```python
n = 3
m = 4

# n x m ũ���� 2���� �迭 0���� �ʱ�ȭ
a = [[0] * m for _ in range(n)] 
print(a) # [[0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0]]

# 1 ~ 9�� �����ϴ� ����Ʈ
a = [i * i for i in range(1,10)]
print(a) # [1, 4, 9, 16, 25, 36, 49, 64, 81]
```

### Ư���� ���Ұ� ��� ����

`remove_set`�� ���Ե� ���ҵ��� ��� ����

```python
arr = [1, 1, 1, 2, 3, 4, 5]
remove_set = {1, 4}

res = [i for i in arr if i not in remove_set]

# for i in arr :
#     if i not in remove_set:
#         res.append(i)
        
print(res) # [2, 3, 5]
```

## ���ڿ�
���ڿ��� ���������� ����Ʈ�� ����.

```python
a = "Hello"
print(a * 3) # HelloHello

print(a[2:4]) # ll
```

## Ʃ�� tuple

- �ѹ� �ʱ�ȭ�� ���� ���� �Ұ�.
- �Ұ�ȣ `()`�� ����Ͽ� ���� ���������� ����.
    - `a = (1, 2 ,3)`
- ����Ʈ�� ���� ���� ȿ�����̴�.
- ���δٸ� ������ �����͸� ��� �����ϴµ� �ַ� ����.

## ���� �ڷ��� dict$($)

- �����͸� **key - value** ���·� ����.
- ���������� �ؽ� ���̺��� �̿�.
- �������� �˻��� ����. `O(1)`

**���� �Լ�**

**`keys()` :** Ű �����͸� ���� ����Ʈ ����

**`values()` :** �� �����͸� ���� ����Ʈ ����

```python
data = dict()
data['����'] = 'Math'
data['����'] = 'Music'
data['����'] = 'Science'

key_list = data.keys()
print(key_list)

data.pop('����')

key_values = data.values() 
print(key_values) # ['Music', 'Science']

data.clear()

if '����' in data : # ������ �������� ����.
    print("������ �����մϴ�.")
```

## ���� �ڷ��� set$($)

- �ߺ��� ������� �ʴ´�.
- ������ ����. `�ε����� ���� ���� ã�� ���Ѵ�.`
- Ư�� �����Ͱ� ������ �� �ִ��� �����Ǵܿ� ����.

### ����

- **������ :** `|`
- **������ :** `&`
- **������ :** `-`

**`update()` :** �������� ���� �ѹ��� �߰�

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

## ���̽��� ������
���� �����͸� �����ϰ� �ִ� �ڷ����� Ư�� ���� ���� ���θ� Ȯ���ϴ� �����ڷ� `in`�� `not in` �����ڸ� �����Ѵ�.

- **`X in ����Ʈ` :** ����Ʈ�� `X`�� �����ϸ� True.
- **`X not in ���ڿ�` :** ���ڿ��� `X`�� �������� ������ True.

## �������� global
�Լ����� ���������� �������� �ʰ� �ۿ� ����� ������ ����.

```python
sum = 0

def func(num) :
    global sum # sum �������� ����
    sum += num;

for i in range(11) : # 1 ~ 10�� ��
    func(i)
    
print(sum) # 55
```

## �����

`input()` : ������ ���ڿ��� �Է¹޴´�.

**�� �ٿ� �ϳ��� �����͸� �Է¹޴� ��� :**

`int(intput())`

**�����Ͱ� ���ٿ� �������� ���еǾ� �ԷµǴ� ��� :**

`list(map(int, input().split()))`

- `input()`���� ���ڿ� �Է¹���.
- `.split()` ���� ������ �������� ���ڿ� �Ľ�
- `map()`�� ����Ͽ� �Ľ̵� ����Ʈ�� ��� ���ҿ� `int()` �Լ� ����.
- `list()`�� ����Ʈ ���·� ����.

**�� ���� �Է¹�� :**

`import sys`
`sys.stdin.readline().rstrip()`

**���**

`f-string` ������ ���� `{}`�ȿ� ������ �־ ����ȯ ���� ���ڿ��� ������ ���� ����� �� �ִ�.

```python
import sys
num = int(input()) # ������ �ϳ� �Է�

data = list(map(int, input().split())) # �������� �����Ͽ� �Է�
n, m, k = map(int, input().split())

fdata = sys.stdin.readline().rstrip()

print(f"num�� {num}�Դϴ�.") # num�� 2�Դϴ�.
```
