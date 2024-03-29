---
toc : true
title : "[C++] 문자열 파싱 함수 - istringstream, fprintf"
category : 
    - C++
---
평소 백준 문제만 풀다가 프로그래머스에서 코딩테스트 문제를 풀며 C++의 문자열 파싱에 애를 먹었다.

문제의 입력값이 대부분 문자열 배열 형태로 주어졌고 이를 파싱하는데 사용했던 문자열 파싱 방법 두가지에 대해서 소개해보고자 한다.

### 헤더 정보
`#include<sstream>`

세 함수 모두 `sstream` 헤더에 포함되어 있다.

### 1. istringstream
문자열을 파싱하여 변수의 형식에 맞게 변환해준다.

**공백을 기준으로 파싱**

```cpp
istringstream iss("test 123 aaa 456");
string s1, s2;
int n1, n2;

iss>>s1>>n1>>s2>>n2; // 공백을 기준으로 순서대로 문자열을 parsing하고 변수 형식에 맞게 변환

cout<<s1<<'\n'; // test
cout<<n1<<'\n'; // 123
cout<<s2<<'\n'; // aaa
cout<<n2<<'\n'; // 456
```

**특정 문자를 기준으로 파싱**

```cpp
string lines = "1998:04:25";
istringstream iss(lines);
string token;
while(getline(iss, token, ':')) cout<<token<<' '; // 1998 04 25
```
**구분자 기준으로 파싱후 배열에 저장**

```cpp
//input
string str("abc:def");
char split_c = ':';

//logic
istringstream split(str);
vector<string> tokens;
for(string each; getline(split, each, split_c); tokens.push_back(each));
```

### 2. fprintf를 통한 문자열 파싱
string 클래스 내의 c_str 메소드를 사용하여 문자열을 파싱한다.

**공백 기준 파싱**
```cpp
string str = "100 101 102";
int n1, n2, n3;
fprintf(str.c_str(), "%d %d %d", n1, n2, n3);
```

**특정 문자 기준 파싱**
```cpp
string str = "100:101:102";
int n1, n2, n3;
fprintf(str.c_str(), "%d:%d:%d", n1, n2, n3);
```
