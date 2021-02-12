---
toc : true
title : "[C++] 포인터 없이 map으로 이진트리 만들기 $($+ 전위, 중위, 후위 순회)"
category :
    - TIL
    - C++
---
C++에서 트리 자료구조를 구현하는 방법은 크게 두가지로 나뉩니다. 완전이진트리일 경우의 배열을 사용하는 방법과 이진트리일 경우의 연결리스트$($포인터)를 사용하는 방법이 있습니다. 

이때 이진트리를 연결리스트로 구현하게 되면 Node라는 class 객체를 따로 만들어서 구현해 주어야 하는데 이는 포인터를 사용하기 때문에 복잡하고 알고리즘 문제를 풀때 번거롭다는 단점이 있습니다.

하지만 C++의 STL인 `map`과 `pair`를 잘 활용한다면 이진트리를 쉽게 구현할 수 있습니다.

### map을 활용한 이진 트리의 구현

다음과 같은 이진트리가 주어졌다고 하자

![이진트리](/assets/images/algo/binary-tree-map.png)

트리를 저장할 `map` 객체를 선언하고 데이터값은 문자이기 때문에 char형으로 선언했습니다. 
또한 `pair`에는 각각 왼쪽 자식 노드와 오른쪽 자식 노드를 집어 넣습니다.

`map<char, pair<char, char> > tree;`

부모 노드 A의 왼쪽 자식은 B, 오른쪽 자식은 C이므로 A노드를 다음과 같이 만들어 줍니다.

`tree['A'] = make_pair('B', 'C');`

B와 같이 왼쪽자식은 존재하지만 오른쪽 자식은 존재하지 않을땐 '.'으로 표현 하겠습니다.

`tree['B'] = make_pair('D', '.');`

D의 경우에도 마찬가지로 왼쪽, 오른쪽 자식에 '.'을 넣어줍니다.

`tree['D'] = make_pair('.', '.');`

위와 같이 진행해 가며 트리에 값을 삽입해 나가면 이진 트리가 완성됩니다.

이렇게 완성된 이진 트리의 A노드의 왼쪽 자식을 알고 싶으면 

`tree['A'].first;`

오른쪽 자식을 알고 싶으면

`tree['A'].second;`

를 통해 접근할 수 있습니다.

### map을 활용한 트리 순회

이렇게 map으로 구현한 이진 트리를 전위, 중위, 후위 순회 하는 방법은 아래의 소스코드를 참고 하기 바랍니다.

``` cpp
#include<iostream>
#include<map>
using namespace std;

map<char, pair<char, char> > tree;

void preOrder(char node) // root, left, right
{
    cout<<node;
    if(tree[node].first!='.') preOrder(tree[node].first);
    if(tree[node].second!='.') preOrder(tree[node].second);
}

void inOrder(char node) // left, root, right
{
    if(tree[node].first!='.') inOrder(tree[node].first);
    cout<<node;
    if(tree[node].second!='.') inOrder(tree[node].second);
}

void postOrder(char node) // left, right, root
{
    if(tree[node].first!='.') postOrder(tree[node].first);
    if(tree[node].second!='.') postOrder(tree[node].second);
    cout<<node;
}

int main(void)
{
    ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
    int n;
    char left, right, data;

    cin>>n;
    for(int i=0; i<n; i++)
    {
        cin>>data>>left>>right;
        tree[data] = make_pair(left, right);
    }

    preOrder('A');
    cout<<'\n';
    inOrder('A');
    cout<<'\n';
    postOrder('A');
    cout<<'\n';

    return 0;
}
``` 

### 풀어보면 좋은 문제
- [트리 순회 $($BOJ_1991)](https://www.acmicpc.net/problem/1991)







