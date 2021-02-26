---
toc : true
title : "[Algorithm] lower_bound, upper_bound 활용"
category : 
    - TIL
    - Algorithm
---
c++의 algorithm 헤더파일은 이진 탐색으로 원소를 탐색하는 `lower_bound`와 `upper_bound` 함수를 제공합니다.

### lower_bound

`lower_bound(탐색 시작 구간, 탐색 끝 구간, key값);`

**lower_bound** : 찾는 key 값과 같은 숫자가 배열 몇 번째에서 처음 등장하는지 찾아서 iterator를 반환 $($찾는 key값이 배열에 존재하지 않으면 key값보다 크면서 가장 가까운 수를 찾음.)

`단 탐색을 진행하는 배열은 오름차순으로 정렬되어 있어야 한다는 조건이 있습니다`

### lower_bound 사용 예제

``` cpp
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

int main(void)
{
    //배열 예제
    int arr[6] = { 1,2,3,4,5,6 }; //오름차순 정렬 되어 있음
    cout<< "lower_bound(6) : "<< lower_bound(arr, arr+6, 6) - arr;
    //O/P : lower_bound(6) : 5

    //벡터 예제
    vector<int> arr = { 1,2,3,4,5,6 };
    cout<< "lower_bound(6) : "<< lower_bound(arr.begin(), arr.end(), 6) - arr.begin();
    //O/P : lower_bound(6) : 5

    return 0;
}
```

위의 예제는 arr배열의 시작 인덱스 부터 끝까지 탐색하면서 6, 혹은 6 이상의 숫자가 처음으로 나오는 위치의 인덱스 번호를 반환합니다.

실제로 arr배열에서 key값인 `6`은 `5번째 인덱스`에서 처음으로 발견됩니다.

lower_bound의 반환형은 iterator이기 때문에 실제로 몇 번째 인덱스인지를 알고 싶다면, 위의 예제와 같이 lower_bound 값에서 배열 첫 번째 주소를 가리키는 배열의 이름, 혹은 arr.begin$($)을 빼주면 됩니다.

### upper_bound

`upper_bound(탐색 시작 구간, 탐색 끝 구간, key값);`

**upper_bound** : 찾으려는 key값을 초과하는 숫자가 배열 몇 번째에서 처음 등장하는지 찾아서 iterator를 반환

`단 탐색을 진행하는 배열은 오름차순으로 정렬되어 있어야 한다는 조건이 있습니다`

### upper_bound 사용 예제

``` cpp
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

int main(void)
{
    vector<int> arr = { 1,2,3,4,5,6 };
    cout<< "upper_bound(3) : " << upper_bound(arr.begin(), arr.end(), 3) - arr.begin();
    //O/P : upper_bound(3) : 3

    return 0;
}
```

배열의 시작부터 끝까지 탐색하면서 key값인 3을 처음으로 초과하는 숫자가 나오는 위치의 인덱스 번호를 반환하는 예제입니다.

3을 처음으로 초과하는 숫자는 `4`이며 4는 배열의 `3번째` 인덱스에 위치해 있습니다.

### 활용

1. **오름차순으로 정렬된 자료에서 특정 범위에 속하는 자료들이 총 몇 개 있는지를 탐색하고자 할 때.**

ex$)$ { 1,2,4,4,5,6,6,6,8,11,14 } 에서 6이상 11 이하의 자료가 총 몇 개인지 구하는 경우

``` cpp
int main(void)
{
    vector<int> arr = { 1,2,4,4,5,6,6,6,8,11,14 };
    cout<< "6이상 11이하의 갯수 : " << upper_boud(arr.begin(), arr.end(), 11) - lower_bound(arr.begin(), arr.end(), 6);

    return 0;
}
```

2. **오름차순으로 정렬된 자료에서 특정한 숫자가 몇 번 나오는지 탐색하고자 하는 경우**

ex$)$ 주어진 배열에서 6이 총 몇 번 나오는지 탐색

``` cpp
int main(void)
{
    vector<int> arr = { 1,2,4,4,5,6,6,6,8,11,14 };
    cout<< "6의 갯수 : " << upper_boud(arr.begin(), arr.end(), 6) - lower_bound(arr.begin(), arr.end(), 6);
    // 6이상 6이하의 자료들을 찾음

    return 0;
}
```

### 풀어보면 좋은 문제
- [합이 0인 네 정수$($BOJ_7453)](https://www.acmicpc.net/problem/7453)

