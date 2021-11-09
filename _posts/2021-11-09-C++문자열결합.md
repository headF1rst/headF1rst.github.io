---
toc : true
title : "[C++] 문자열 결합 append, insert, + 사용법"
category : 
    - C++
---
C++에서 문자열을 결합하는 방법에는 **append, insert** 그리고 **+ 연산자**가 있습니다.

### + 연산자
```cpp
string str1 = "ab";
string str2 = "cd";

cout<<(str1 + str2)<<'\n'; // abcd
cout<<(str1 + "cd")<<'\n'; // abcd
cout<<("ab" + "cd")<<'\n'; // 에러!!!!
```

이때 cout<<("ab" + "cd")<<'\n'; 처럼 문자열 리터럴을 결합하는 경우에는 에러가 발생한다는 점을 주의해야 한다.

### insert 함수
insert 함수는 지정한 위치에 문자열을 추가한다.
인덱스는 0부터 시작한다.

```cpp
string str1 = "abcde";
string str2 = str1.insert(2, "Z");
cout<<str2<<'\n'; // abZcde
```

### append 함수
`.append(문자열)`

`.append(문자열, 문자수)`

`.append(문자열, 위치, 문자수)`

```cpp
string str1 = "abcd";
string str2 = str.append("ABC");
cout << str2 << endl; //abcdABC

string str1 = "abcd";
string str2 = str.append("ABC", 1);
cout << str2 << endl; //abcdA

string str1 = "abcd";
string str2 = str.append("ABC", 0, 2);
cout << str2 << endl; //abcdAB
```
