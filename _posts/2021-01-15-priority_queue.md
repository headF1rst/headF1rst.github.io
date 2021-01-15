---
toc : true
title : "[STL] 우선 순위 큐 priority_queue"
category : 
    - TIL
    - STL
---
우선 순위 큐 `(priority queue)`에서 데이터의 삽입은 일반적인 큐와 같지만 삭제는 우선 순위가 가장 높은 것을 먼저 삭제하는 특징이 있다. 삽입된 데이터는 트리구조인 `히프 (heap)`를 사용하여 주어진 조건에 맞는 우선 순위 순으로 루트$($top)에서 부터 아래로 정렬된다. 

### [STL] 우선 순위 큐의 멤버함수 및 사용
`#include<queue>`

`priority_queue<int, vector<int>, less<int> > pq;`

우선순위 큐는 실제로는 priority_queue<자료형, 구현체, 비교 연산자>로 정의하는 것을 알 수 있다. 
비교 연산자를 `less<int>`로 선언하면 큰 값부터 출력되고 `greater<int>`로 선언하면 작은 값 부터 출력된다. 비교 연산자는 위와 같이 이미 정의된 연산자 뿐만 아니라 비교 연산을 수행하는 구조체를 직접 작성하여 사용하는것도 가능하다. <br>

우선순위 큐의 멤버함수는 일반 큐와 동일하므로 바로 예제를 통해 설명하겠다.

``` cpp
#include<iostream>
#include<queue>
#include<vector>
using namespace std;

int main(void)
{
    priority_queue<int, vector<int>, less<int> > pq;
    pq.push(3);
    pq.push(1);
    pq.push(4);
    pq.push(2);
    pq.push(5);
    pq.push(6);

    while (!pq.empty()) 
    {   // 출력 : 6 5 4 3 2 1
        cout<<pq.top()<<' ';
        pq.pop();
    }

    return 0;
}
```

구조체로 비교연산자를 구현하여 사용한 경우
``` cpp
#include<iostream>
#include<queue>
#include<vector>
using namespace std;

typedef struct 
{
    int start;
    int end;
    int value;
} c;

typedef struct 
{
    bool operator()(c t, c u) 
    {
        return t.value < u.value; //value가 큰값 부터 출력
    }
} cmp;

int main(void)
{
    priority_queue<c,vector<c>,cmp> pq;
    pq.push(10,20,3);
    pq.push(30,40,1);
    pq.push(50,60,4);
    pq.push(70,80,2);
    pq.push(90.100,5);
    pq.push(110,120,6);

    while (!pq.empty()) 
    {   // 출력 : 6 5 4 3 2 1
        cout<<pq.top()<<' ';
        pq.pop();
    }

    return 0;
}
```

`priority_queue<pair<int,int>, vector<int, int>, greater<int,int> >` 와 같이 pair의 사용도 가능하다.


