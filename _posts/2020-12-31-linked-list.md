---
toc: true
title: "[STL] 연결 리스트 Linked-list"
category: 
    - STL
---
오늘은 데이터를 순차적으로 저장하는데 사용되는 자료구조인 연결 리스트에 대해서 알아보겠습니다.
기본적으로 연결 리스트는 배열과 같은 용도로 사용되지만 `index`값에 의해 접근되는 배열과 달리 `link` 포인터를 활용해서 접근한다는 차이점이 있습니다. 이러한 특징때문에 연결 리스트는 `중간에 새로운 값을 삽입하거나 삭제하는 경우에 유용하게 사용됩니다.`

원소의 삽입과 삭제에서 배열의 경우 최대 `O(n)` 이 소요되는 반면 연결리스트의 경우 `상수시간` 밖에 소요되지 않는 장점이 있습니다.
![linked-list](/assets/images/linked-list.png "단순 연결 리스트")

하지만 위의 단순 연결리스트의 경우 전위 노드를 찾기 힘들다는 단점이 있으며 이를 보완하기 위해 양방향 연결 필드를 갖는 `이중 연결 리스트 (Double linked list)`를 주로 사용합니다.

### 이중 연결 리스트 $($Double linked list)

![DblLinked-list](/assets/images/DblLinked-list.png "이중 연결 리스트")

알고리즘을 간편하게 작성하기 위해서 `헤드 노드` 를 추가하며 이 경우 이중 원형 연결 리스트가 됩니다. 이때 헤드노드는 데이터를 저장하기 위한 노드가 아닌 함수를 간편하게 작성하기 위한 노드 이므로 Data field 는 아무 의미가 없습니다.
> **Right Link : 후위 노드를 가리킴.**<br>
**Left Link : 전위 노드를 가리킴.**

C++에서의 STL `list`또한 `이중 연결 리스트` 로 구현되어 있습니다.

### [ADT] 연결리스트의 구현
``` cpp
#include<iostream>
using namespace std;

class DblList; //전방선언
class DblListNode //이중 연결 리스트를 구성하는 노드 
{
    //DblListNode class의 멤버 변수에 접근하기 위한 friend선언
    friend class DblList; 
private:
    int data; //데이터 필드
    DblListNode *left, *right;
};

class DblList
{
private:
    DblListNode *first; //헤드 노드를 가리킴
public:
    // + 리스트 조작 연산 (생성자 함수 등...)
    void Insert(DblListNode *p, DblListNode *x);
    void Delete(DblListNode *x); //x는 삭제하는 노드의 포인터값
};

void DblList::Insert(DblListNode *p, DblListNode *q)
{
    //노드 p를 노드 x의 오른쪽에 삽입한다.
    p->left = x; p->right = x->right;
    x->right->left = p; x->right = p;
}

void DblList::Delete(DblListNode *x)
{
    if(x==first) //참이면 헤드 노드의 삭제를 의미
        cerr<< "Deletion of headnode not permitted\n";
    else
    {
        x->left->right = x->right;
        x->right->left = x->left;
        delete x;
    }
}
```

### 이중 연결 원형 리스트에서의 <u>삽입</u>

![DblLinked-list-Insert](/assets/images/DblLinked-list-Insert.png "이중 연결 리스트 삽입")

### 이중 연결 원형 리스트에서의 <u>삭제</u>

![DblLinked-list-Delete](/assets/images/DblLinked-list-Delete.png "이중 연결 리스트 삭제")

### [STL] list의 멤버함수 및 사용
`#include<list>` 
<br>

`list<[Data type]> [변수이름];`
<br>

ex) list<int> ls;

**Iterator (반복자)**

- **begin$($)** : beginning iterator 반환

- **end$($)** : end iterator 반환

**삽입 및 삭제**

- **push_front$($element)** : 리스트 맨앞에 원소 추가

- **push_back$($element)** : 리스트 맨뒤에 원소 추가

- **pop_front$($element)** : 리스트 맨앞 원소 삭제

- **pop_back$($element)** : 리스트 맨뒤 원소 삭제

- **insert$($iterator, element)** : iterator가 가리키는 부분의 앞에 원소 추가

- **erase$($iterator)** : iterator가 가리키는 부분의 원소 삭제

- **remove$($element)** : element와 같은 원소들을 전부 삭제

- **remove_if$($predict)** : 단항 조건자 predicate에 해당하는 원소를 모두 제거

**Search**

- **front$($)** : 맨앞의 원소를 반환

- **back$($)** : 맨뒤의 원소를 반환

- ***iterator** : iterator가 가리키는 원소에 접근

**ETC**

-  **empty$($)** : 리스트가 비었으면 true, 아니면 false 반환

- **size$($)** : 리스트의 총 원소수를 반환

- **assign$($num_element, value)** : num_element 개의 원소를 할당하고 value값으로 초기화한다.

**사용 예제**

``` cpp
#include <iostream>
#include <list>
using namespace std;

int main(void)
{
	list<int> l;

	// push_back
	l.push_back(3);
	l.push_back(4);
	l.push_back(5);

	// pop_back
	l.pop_back(); //5가 삭제

	// push_front
	l.push_front(1);
	l.push_front(0);

	// pop_front
	l.pop_front(); //0이 삭제

	// back and front
	cout << "list front value: " << l.front() << '\n';
	cout << "list end value: " << l.back() << '\n';

	// size
	cout << "list size: " << l.size() << '\n';

	// empty
	cout << "Is it empty?: " << (l.empty() ? "Yes" : "No") << '\n';

	// iterator
	list<int>::iterator begin_iter = l.begin(); // auto begin_iter = l.begin()도 가능
	list<int>::iterator end_iter = l.end(); // auto end_iter = l.end()도 가능

	// insert
	begin_iter++; // 2번째를 가리키는 iterator
	l.insert(begin_iter, 2);

	// erase
	end_iter--; // 마지막 원소를 가리키는 iterator
	l.erase(end_iter);

	// *iterator: 원소 접근
	cout << "list "<< distance(l.begin(), begin_iter)+ 1 << " element: " << *begin_iter << '\n';

	return 0;
}
```

### 연결 스택과 큐
``` cpp
#include<stack>
#include<queue>
#include<list>
using namespace std;

stack<list<int> > ls_st;
queue<list<int> > ls_q;
```
와 같이 연결 리스트로 구현된 스택과 큐를 활용할수 있습니다.
원소의 삽입 위치가 자유롭다는 점에서 유용하게 사용됩니다.

