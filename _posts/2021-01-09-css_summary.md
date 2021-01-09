---
toc : true
title : "CSS 문법 및 활용"
category :
    - Front_End
---
CSS는 HTML 문서에 색, 모양, 출력 위치 등 외관을 꾸미는 언어이며, CSS로 작성된 코드를 "style sheet"라고 합니다.

### CSS 스타일 시트 구성
![CSS의구성](/assets/images/프론트엔드사진/css구성.png)

위의 스타일 시트는 HTML 페이지의 모든 <h3> 태그에 텍스트 색은 "blue", 택스트 크기는 디폴트값의 20배의 스타일을 적용할 것을 의미합니다.

### CSS 스타일 시트 작성
CSS 스타일 시트 작성법에는 `<style>` 태그에 작성하거나 style 속성에 작성 할 수도 있지만 스타일 시트를 별도의 파일에 작성하고 `<link>` 태그로 외부의 css파일을 불러서 사용하는 방법에 대해 알아보겠습니다.

``` css
/* mystyle.css*/
h3 {text-align:center; color:blue;}
``` 

``` html
<!--.html-->
<head>
    <link href="mystyle.css" type="text/css" rel="stylesheet">
</head>
```
link 태그를 통한 스타일 시트 작성 방법은 동일한 CSS 스타일을 웹 페이지 마다 중복 작성하는 불편함을 해결해 주고 웹 사이트 전체 웹 페이지 모양에 일관성을 줄 수 있습니다.

### 셀렉터
셀렉터$($selector)는 특정 HTML 태그의 모양을 꾸밀 스타일 시트를 선택하는 기능입니다. 위의 h3처럼 태그 이름이 셀렉터가 되기도 하고 id 속성이나 class 속성 값을 셀렉터로 사용하기도 하며 여러 셀렉터를 조합하여 사용하기도 합니다. 여기서는 class 셀렉터에 대해서 알아보겠습니다.

**class 셀렉터**
셀렉터 이름 앞에 점을 붙인 경우, 이 셀렉터는 HTML 태그의 `class`속성으로만 지정할 수 있습니다. 또한 class 셀렉터는 class 속성이 같은 모든 태그에 적용됩니다.
``` css
.warning {color : blue;} /* css */
```

``` html
<body>
    <div class="warning">조심하세요!</div>
</body>
```
`body.main` 과 같이 태그 이름과 함께 만들어 진 경우엔 해당 태그에만 제한되어 사용되게 됩니다. 즉, class="main"을 div 같은 다른 태그에선 사용 불가 합니다.

**전체 셀렉터**
(*) 문자를 사용하여 웹 페이지의 모든 태그에 해당 스타일을 적용. 
``` css
*{color : blue;} /* css */
```
**속성 셀렉터**
HTML 태그의 특정 속성의 속성값에 일치하는 태그에만 스타일 적용.
``` css
input[type=text] {color : blue;} /* type 속성값이 "text"인 <input> 태그에 적용 */
```
**가상 클래스 셀렉터**
어떤 상황이 발생하였을 때만 적용하도록 만들어진 셀렉터.
여러가지가 있지만 대표적으로 `:hover`의 경우 특정 태그 위에 마우스가 올라오면 그 태그에 스타일을 적용시킵니다.
``` css
li:hover {background : blue;} /* <li> 태그에 마우스 올라가면 배경화면을 blue로 출력. 마우스 내려가면 원래대로 복귀 */
```
### 색과 텍스트 꾸미기
CSS에서 색은 16, 10진수 코드로 표현되거나 색 이름으로 표현되지만 이중 가장 선호되는 방식은 색 이름으로 표현하는 것 입니다.

**색 관련 프로퍼티**
``` css
태그 {
color : 색; /* HTML 태그의 텍스트 글자색 */
background-color : 색; /* HTML 태그의 배경 색 */
border-color : 색; /* HTML 태그의 텍스트 테두리 색 */
}
```  
**택스트 프로퍼티**
``` css
태그 {
text-indent : <length> | <percentage>;/* 들여쓰기 */
text-align : left | right | center | justify; /* 정렬 */
text-decoration : none | underline | overline | line-through; /* HTML 태그의 텍스트 테두리 색 */
}
```  
text-indent에서 `<length>`는 3px 같이 고정된 길이이고 `<percentage>`는 텍스트 블록 전체 폭에 대한 비율로 들여쓰기 하는 방식 입니다. text-align에서 justify는 양쪽정렬을 의미 합니다.

**폰트 이름 지정 font-family**
font-family 프로퍼티에는 텍스트에 사용할 폰트 이름을 지정할수 있습니다. 만약 해당 폰트가 브라우저에서 지원하지 않을 경우 뒤에 나열된 폰트 순으로 적용됩니다. 

ex$)$Arial을 지원하지 않으면 "Times New Roman" 사용. 폰트 이름에 빈칸이 있으면 " "로 묶어서 표현.
``` css
font-family : Arial, "Times New Roman", Serif:
```
**폰트 크기, font-size**
``` css
font-size : 1.6em; /*현재 폰트의 1.6배 크기로 설정*/
``` 
**폰트 스타일, font-style**
``` css
font-style : italic; /* 이탤릭 스타일로 지정 */
``` 
**폰트 굵기, font-weight**
``` css
font-weight : 300; /* 100~900 범위에서 300 정도 굵기*/
font-weight : bold; 
```
### CSS 박스 모델
CSS에서는 각 HTML 태그 요수를 하나의 박스로 간주하며 박스는 4개의 영역으로 구성되어 있습니다.
![박스구성](/assets/images/프론트엔드사진/박스구성.png)

``` css
div.box {
    width : 150px; /* 콘텐츠 */
    height : 50px; /* 콘텐츠 */
    margin : 40px; /* 여백의 두께 */
    border-width : 30px;  /* 보더의 두께 */
    padding : 20px;  /* 패댕의 너비 */
}
``` 

**박스 제어 프로퍼티**
![박스제어프로퍼티](/assets/images/프론트엔드사진/박스프로퍼티.png)

``` css
p {
    border-width : 3px; /* 테두리 두께 3픽셀 */
    border-style : dotted; /* 테두리 점선 */
    border-color : blue; /* 테두리 blue 색 */
}
``` 

**둥근 모서리 테두리, border-radius**
`border-radius`프로퍼티를 이용하여 테두리의 모서리를 둥글게 만들 수 있습니다.
``` cpp
{ border-radius : 50px; /*네 모서리 모두 반지름이 50px인 둥근 모서리로 만들어줌*/}

{ border-radius : 0px 20px 40px 60px;}
``` 
두번째의 경우 왼쪽 맨 위의 모서리 부터 시계방향 순으로 해당 반지름을 적용 시켜줍니다.

**이미지 테두리, border-image**
`border-image`프로퍼티를 이용해 테두리를 모서리$($corner)와 에지$($edge)로 구분하여 이미지를 입힐 수 있습니다. 단 border-image는 border-style 대신 지정되기 때문에 다음의 프로퍼티가 먼저 지정되어 있어야 합니다.
``` cpp
{ border-width : 30px; /*테두리 폭 지정*/}
{ boder-style : solid; /*테두리 스타일 지정*/}
``` 

``` cpp
{ border-image : url("border.png") 30 round;}
```
위의 border-image 프로퍼티는 "border.png" 이미지를 30x30 픽셀 크기로 잘라서 4개의 모서리와 4개의 에지 이미지를 만든후 round 에 의해 에지에 이미지가 반복 배치되게 됩니다.

- round - 테두리 길이만큼 에지 이미지 크기 자동 조절, 에지 이미지 반복 배치
- repeat - 이미지 크기 조절 안됨. 에지 이미지 반복 배치
- stretch - 에지 이미지를 테두리 길이만큼 늘여 배치

**배경**
- `background-color : skyblue;` - 배경 색을 지정
- `background-image : url("media/dog.png");` - 배경 이미지를 지정
- `background-position : center center;` - 박스 내 배경 이미지의 시작 위치를 지정한다.
- `background-size : 100px 100px;` - 배경 이미지를 원하는 크기로 출력. 위 경우엔 100x100 크기로 출력.
- `background-repeat : repeat-y;` - 배경 이미지를 반복 출력하는 방식을 지정. repeat, repeat-x $($x축 방향으로 반복 출력), repeat-y

### 시각적 효과

**텍스트 그림자, text-shadow**
text-shadow 프로퍼티를 이용하면 텍스트에 그림자 효과를 줄 수 있습니다. 
`text-shadow : h-shadow v-shadow blur-radius color|none`
- h-shadow, v-shadow : 원본 텍스트와 그림자 사이 수평/수직 거리$($필수)
- blur-radius : 그림자가 흐릿하게 번지는 길이
- color : 그림자 색 
- none : 그림자 효과 없음

``` cpp
div.blur {
    text-shadow : 3px 3px 5px red;
}
```
**박스 그림자, box-shadow**
box-shadow 프로퍼티를 이용하면 박스 전체에 그림자 효과를 줄 수 있습니다. 
`box-shadow : h-shadow v-shadow blur-radius spread-radius color | none | inset`
- spread-radius : 그림자 크기
- inset : 음각 박스로 보이게 그림자 형성