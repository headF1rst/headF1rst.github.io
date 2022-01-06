---
layout: post
toc: true
title: "[C++] Undefined behavior $($cout과 printf의 출력 순서)"
date:
lastmode:
sitemap:
category: 
    - C++
---
오늘은 C++의 Undefined behavior와 Unspecified behavior에 의한 결과 값의 차이를 알아보겠습니다.

`cout<<fun1<<fun2;` 의 수행결과가 제가 예상한 `fun1->fun2` 순으로 출력되지 않고 `fun2 -> fun1` 순서로 출력되는 것을 보고 왜 이런 결과가 나오게 되었는지 알아보게 되었습니다.

다음과 같은 코드의 출력값을 예상해봅시다.

```cpp
#include <iostream> 
using namespace std; 
int c = 6;
int f() 
{   
       c+=1; 
       return c; 
} 

int main() 
{ 
        int i = 0; 
        cout <<"i="<<i<<" i++="<<i++<<" i--="<<i--<<endl; 
        i = 0;
        printf("i=%d i++=%d i--=%d\n" , i , i++ ,i-- );

        cout<<f()<<" "<<f()<<" "<<f()<<endl; 
        c = 6;
        printf("%d %d %d\n" , f() , f() ,f() );
        system("pause");
        return 0; 
} 
```
위의 코드를 VS2005와 g++로 컴파일 해보았습니다.

## Under 'vs2005'

```cpp
i=0 i++=-1 i--=0
i=0 i++=-1 i--=0
9 8 7
9 8 7
```

## Under 'g++'

```cpp
i=0 i++=0 i--=1
i=0 i++=-1 i--=0
9 8 7
9 8 7
```
왜 두개의 다른 컴파일러에서 다른 결과값이 나온것일까요?

그 이유중 하나는 위의 printf 함수의 `,` 사이에 sequence point가 없어서 Undefined 되어있기 때문입니다.

sequence point를 사이에 두지 않고 하나의 변수의 값을 두번 이상 변경하는 경우 Undefined behavior의 예에 속합니다. 예를들어 `i = i++` 의 경우 변수 `ì` 가 두변 변하는 사이에 sequence point가 존재하지 않기 때문에 undefined 이라고 볼수있습니다. 즉 이러한 undefined behavior 때문에 printf 인자의 실행순서가 명확하지 않고 컴파일러에 따라 다른 순서로 수행하게 되면서 다른 결과가 출력되게 된것입니다.

---

## Sequence point
>시퀀스 포인트는 컴퓨터 프로그램의 실행에서 이전 평가의 모든 부작용이 수행되고 후속 평가의 부작용이 아직 수행되지 않았 음을 보증합니다. 

- Between the evaluations of the function designator and actual arguments in a function call and the actual call;
<br>
- Between the evaluations of the first and second operands of the operators ```&&```, ```||```, and ```,```;
<br>
- Between the evaluations of the first operand of the conditional ```?:``` operator and whichever of the second and third operands is evaluated;
<br>
- The end of a full declarator;
<br>
- Between the evaluation of a full expression and the next full expression to be evaluated. The following are full expressions:
    - an initializer;
    - the expression in an expression statement;
    - the controlling expression of a selection statement (```if``` or ```switch```);
    - the controlling expression of a ```while``` or do statement;
    - each of the expressions of a ```for``` statement;
    - the expression in a return statement.
<br>
- Immediately before a library function returns;
<br>
- After the actions associated with each formatted input/output function conversion specifier;
<br>
- Immediately before and immediately after each call to a comparison function, and also between any call to a comparison function and any movement of the objects passed as arguments to that call.

---
그렇다면 cout의 경우에는 어떨까요? 

위의 코드에서 cout은 총 세번의 `operator >> ` 함수 호출을 하고 있습니다. 

함수 호출의 경우 sequence point를 사이에 두고있기 때문에 `i` 메모리 공간에 left to right 의 정의된 순서를 갖고 접근합니다. 

하지만 C/C++에서는 함수 인자의 evaluate 순서가 정해져 있지 않기 때문에 cout이 함수여도 컴파일러에 따라 다른 결과 값을 얻게 되는 것입니다. 

`cout<<i<<i++<<i--` 는 `cout.operator<<(i).operator<<(i++).operator(i--)`와 같고 이는 `f(h(g(cout, i), i++), i--)` 와 같다고 볼수있습니다. 

따라서 함수의 인자들이 unspecified order에 의해 evaluate되기 때문에 수행되는 컴파일러에 따라 다른 값을 얻게 되는 것입니다.
