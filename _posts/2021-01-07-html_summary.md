---
toc : true
title : "HTML 문법 및 활용"
category : 
    - Front_End
---
웹페이지는 총 3가지 언어를 사용하여 구성되어집니다. `HTML` 은 웹페이지의 구조 맟 내용을, `CSS` 웹페이지의 모양을, 그리고 `javascript` 는 웹페이지의 동적인 응용프로그램을 작성하는데 사용됩니다. 
이중 오늘은 HTML에 대하여 정리해 보았습니다.

### HTML 태그 구성 
HTML은 기본적으로 `<태그>` 들의 집합이라고 할 수 있습니다. 태그는 여러 `속성`들로 구성되며 속성은 또 `속성 이름`과 `속성 값`으로 이루어져 있습니다.

![테그의구성](/assets/images/프론트엔드사진/태그의구성.png)

`<시작 태그>...</종료 태그> but 종료 태그가 없는 경우도 있습니다.`

- `<DOCTYPE html>` : HTML5 문서임을 브라우저에 알리는 지시어
- `<title></title>` : 페이지의 제목으로, 브라우저의 타이틀 바에 출력되며 <head> 내에서만 작성되어야 한다.
- `<h1> ~ <h6>` : 문단의 제목에 주로 사용되며 <h1>에서 <h6> 순으로 텍스트의 크기가 작아진다.
    - `title 속성` : 페이지의 본문에 마우스가 올라가면 설명문$($툴팁)이 출력되게 해준다.
- `<p></p>` : 문단을 단락으로 나눠준다.
- `<hr>`: 수평선을 삽입.
- `<br>`: 새로운 줄로 넘어간다.
- `&엔터티;` : HTML에서 예약어로 사용하고 있어서 입력할 수 없는 기호들을 입력해준다. ex$)$ `<` --> `&lt;`
- `<pre></pre>` : 개발자가 입력한 포맷을 그대로 출력

- `<ol></ol>` : 순서 있는 리스트
- `<ul></ul>` : 순서 없는 리스트
- `<dl></dl>` : 정의 리스트

### 블록 태그와 인라인 태그

- **블록 태그** : 항상 새 라인에서 시작하며 브라우저의 왼쪽 끝에서 오른쪽 끝까지 블록 공간을 차지합니다. 
    - `<div>` : 특별한 의미 없이 여러 HTML 태그들을 블록으로 묶는 컨테이너로 이용됩니다. 주로 블록을 하나의 단위처럼 다루고자 할 때 사용됩니다.

- **인라인 태그** : 블록에 삽입되어 블록 콘텐트의 일부를 표현하는데 사용됩니다.
    - `<span>` : 텍스트 일부분을 제어하고자 할 때 사용됩니다.

### HTML의 메타데이터 삽입
- `<base>` : 웹 페이지들이 담겨 있는 기본 URL과 웹페이지가 출력될 윈도우를 지정하기 위해 사용된다.

``` html
<head>
    <base href="http://www.mysite.com/score/">
</head>

<a href="math.html">수학</a>
<!-- 브라우저는 math.html을 "http://www.mysite.com/score/math.html로 처리한다. --> 
```
- `<meta>` : 메타 데이터를 표현하는데 사용

검색 엔진에 발견되게 하기위한 웹페이지 키워드 등록
``` html
<meta name="keywords" content="컴퓨터, 야구">
```
charset 속성으로 문자의 인코딩 방식 지정
``` html
<meta charset="UTF-8">
```

### 이미지 삽입
`<img>` 태그를 사용하며 `src` 속성에 다른 웹사이트의 이미지 주소 혹은 이미지가 저장된 위치를 속성값으로 주어 HTML문서에 이미지를 삽입 할 수 있습니다.

**속성**

`<img`
- src = "이미지 파일의 URL"
- width = "이미지의 폭"
- height = "이미지의 높이"
- alt = "이미지 출력 불가일때 대신 출력되는 문자열"
``` html
<img src="media/dog.jpg" width="150" height="100" alt="강아지 사진 없음">
```
### 표 만들기 
- `<table>` : 표 전체를 담는 컨테이너
- `<caption>` : 표 제목
- `<thead>` : 헤딩 셀 그룹
- `<tfoot>` : 바닥 셀 그룹
- `<tbody>` : 데이터 셀 그룹
- `<tr>` : 행을 표현. 여러 개의 <td>, <th>포함
- `<th>` : 제목 셀
- `<td>` : 데이터 셀
``` html
<table>
    <caption>1학기 성적</caption>
    <thead>
        <tr><th>이름</th><th>HTML</th></tr>
    </thead>
    <tfoot>
        <tr><th>합</th><th>150</th></tr>
    </tfoot>
    <tbody>
        <tr><td>고만세</td><td>100</td></tr>
        <tr><td>고우울</td><td>50</td></tr>
    </tbody>
</table>
```
### 하이퍼 링크
하이퍼 링크는 `<a>` 태그와 href 속성을 사용하여 다른 웹페이지로의 이동을 가능하게 합니다.

`<a`
- href = "이동할 페이지의 URL 혹은 엥커이름"
- target = "링크에 연결된 페이지가 출력될 윈도우 이름 지정
- download = "링크가 클릭되면 href에 저장된 파일이 다운로드 됨"
``` html
<a href="student/register.html">학생 등록 페이지로 이동</a>
```

이미지에 하이퍼링크를 다는 경우에는
``` html
<a href="https://www.naver.com">
    <img src="naver.png" alt="네이버">
</a>
```

하이퍼링크를 통한 파일 다운로드 경우
``` html
<a href="dog.png" download>클릭하면 강아지 사진 다운로드</a>
```
### id 속성으로 앵커 만들기
우리가 지금 보고있는 페이지의 오른편에 있는 "On this page" 표에서 임의의 제목을 누르면 해당 내용이 작성된 위치로 페이지가 이동하듯 문서 내 특정 위치로 이동하는 링크를 `앵커`라고 합니다.

앵커는 어떤 태그든 id 속성을 사용하여 `id="앵커이름"` 형태로 해당 태그의 위치에 앵커를 만들수 있습니다.

``` html
<h3 id="chap1">제 1장</h3>
```
**앵커에 연결하는 링크 만들기**
`<a href="#앵커이름">`의 형식으로 앵커에 연결하는 하이퍼링크를 만들 수 있습니다.
``` html
<a href="#chap1">제 1장으로 이동</a>
```

### 인라인 프레임
`<iframe>`태그를 이용하여 현재 HTML 페이지 내에 내장 윈도우를 만들수 있고 이러한 내장 윈도우를 인라인 프레임 이라고 합니다.

`<iframe`
- src = "출력할 웹 페이지의 URL주소"
- srcdoc = "직접 HTML로 작성된 텍스트 출력"
- name = "프레임 윈도우의 이름으로 다른 웹 페이지에서 target 속성 값으로 사용"
- width = "프레임의 폭"
- height = "프레임의 높이"

단, /</iframe> 태그는 <body> 태그 내에서만 사용 가능하며 srcdoc 가 있으면 src 속성은 무시됩니다. 또한 인라인 프레임안에 또다른 인라인 프레임을 중첩시킬 수도 있습니다.

이러한 인라인 프레임의 중첩성 때문에 윈도우 사이에는 부모 자식 계층 관계가 형성됩니다.

``` html
<iframe src="http://www.w3c.org" name="frame1"></iframe>
...
<a href="http://www.w3c.org" target="frame1">W3C</a>
```
frame1 이름의 프레임에 http://www.w3c.org 사이트 출력

### 비디오 삽입
`<video` 
- src = "비피오 파일의 URL"
- width = "비디오 재생 영역의 폭"
- height = "비디오 재생 영역의 높이"
- controls : 재생, 재생시간, 중단, 음소거 등 제어 버튼 출력
- autoplay : 비디오 로딩 즉시 재생
- loop : 비디오 반복 재생
- muted : 오디오를 끌 때 사용
``` html
<video src="dog.mp4" width="300" height="100" controls autoplay></video>
```

### 오디오 삽입
`<audio`
- src = "오디오 파일의 URL"
- controls :  재생, 재생시간, 중단, 음소거 등 제어 버튼 출력
- autoplay : 비디오 로딩 즉시 재생
- loop : 비디오 반복 재생
``` html
<audio src="music.mp3" controls autoplay loop></audio>
```

### HTML 문서 구조화와 시맨틱 태그
검색엔진은 웹페이지가 어떤 구조로 이루어져 있는지 인식하기 위해 문서내의 시맨틱 태그를 참고하고 이러한 시맨틱 태그로 구조화되어 내용 탐색에 용이한 웹을 시맨틱 웹이라 합니다.

### 시맨틱 태그의 종류
- `<header>` : 페이지나 섹션의 머리말을 표현
- `<nav>` : 페이지 내 목차를 만들기 위한 하이퍼링크들을 모아 놓은 섹션
- `<section>` : 문서의 장 혹은 절 작성에 사용
- `<article>` : 보조적인 기사등 본문과 연결되어 있지만 독립적인 콘텐츠를 담는 영역으로 사용
- `<aside>` : 본문에서 벗어난 노트나 기사등을 담기 위해 사용되는 영역이며 주로 사이드에 배치
- `<footer>` : 꼬리말, 저작권 정보등을 담는 영역으로 사용

시맨틱 태그는 구조만을 정의하며 위치는 CSS를 사용하여 설정해 주어야 합니다.

**시맨틱 블록 태그**
- `<figure>` : 이미지, 동영상 등 콘텐츠를 블록화 시키며 콘텐츠의 제목은 figure 태그 내에 `<figcaption>`태그로 작성한다.
<br>
- `<details>, <sumary>` : details 태그는 웹페이지를 간소화 시키기 위해 상세 정보를 감추거나 보이게 할 수 있으며 summary 태그는 details 태그로 구성되는 블록의 제목을 표현한다.
``` html
<details>
    <summary>Question 1</summary>
    <p> 웹 개발자가 알아야 할 언어는?</p>
</details>
```
결과

![details_close](/assets/images/프론트엔드사진/details_tag.png)
Question 1 클릭시

![details_open](/assets/images/프론트엔드사진/details_tag_open.png)

**시맨틱 인라인 태그**
- `<mark>` : 중요한 텍스트임을 하이라이트로 표시
- `<time>` : 시간 정보임을 표시
- `<meter>` : 주어진 범위나 %의 데이터 양 표시
``` html
난이도 <meter value="0.8" max="1.0">80%</meter>
```
- `<progress>` : 작업의 진행 정도 표시

### 웹 폼
웹 페이지를 통해 사용자의 입력을 받는 폼을 `웹 폼` 이라 하며 웹 폼을 만들기 위해 제공되는 태그들을 `폼 요소` 라고 합니다.
폼 요소들은 <form> ... </form> 태그에 담겨서 사용됩니다.

### 폼 태그
`<form`
- name = "폼 이름"
- action = "폼 데이터를 처리할 웹 서버 응용프로그램 이름 URL" + submit 버튼이 클릭되면 브라우저는 action 속성에 저장된 웹 서버에 연결하고 웹 서버 응용프로그램을 실행할 것을 지시합니다.
- method = "폼 데이터를 웹 서버에 전송하는 방식. GET|POST"
    - GET : 서버로 보내는 값들이 사용자에게 보인다. name/value 쌍으로 URL에 붙여서 사용됨. 북마크 필요성이 있을때 사용
    - POST : 서버로 보내는 값들이 사용자에게 안보이기 때문에 중요한 값이나 개인정보가 들어있는 경우 사용. URL형태가 아닌 HTTP 메시지 형태로 서버에 전송함.
- enctype = "폼 데이터를 웹 서버로 전송할 때 암호화 방식 지정
- target = "웹 서버 응용프로그램으로부터 전송받은 데이터를 출력할 윈도우 이름"  >

### 폼 요소
다음은 폼 태그 안에서 사용자 입력을 받기 위해 사용되는 폼 요소 들입니다.

**텍스트 입력 `<input type="text|password">, <textarea>`**

`<input`
- type = "text | password"
- name = "요소 이름"
- maxlength = "입력할 수 있는 무자의 최대수"
- size = "입력창의 크기"
- value = "입력창에 초기에 입력되있는 초기 텍스트" >

`<textarea`
- cols = "열 개수 - 텍스트 입력창의 크기"
- rows = "행 개수 - 텍스트 입력창의 크기"
- wrap = "자동 줄바꿈 처리 지정 - OFF|HARD|SOFT"
- name = "요소 이름" >
초기 출력될 텍스트

`</textarea>` 

**데이터 리스트를 가진 텍스트 입력창 `<datalist>`**
`<input>` 태그의  list 속성 값과 `<datalist>`의 id 속성 값을 동일하게 하면 이 둘을 연결할수 있다. 데이터 리스트 테그 내의 `<option>` 태그는 데이터 항목 하나를 나타낸다.
``` html
<form>
나라 : <input type="text" list="countries"><br>
     <datalist id="countries">
        <option value="일본">
        <option value="영국">
        <option value="브라질">
    </datalist>
</form>
``` 
![datalist실행예제](/assets/images/프론트엔드사진/datalistexample.png)
**텍스트/이미지 버튼 만들기**
- **type="button"** : 자바스크립트에 이용되는 기능없는 버튼으로 "로그인 버튼" 문자열이 새겨진 단순 버튼입니다.
``` html
<input type="button" value="로그인 버튼">
``` 
- **type="submit"** : 버튼을 클릭하면 브라우저가 폼 데이터를 웹 서버로 전송합니다.
``` html
<input type="submit" value="전송">
``` 
- **type="reset"** : 폼에 입력된 내용을 모두 초기화 하는 버튼입니다.
``` html
<input type="reset" value="리셋">
``` 
- **type="image"** : 이미지를 가진 버튼으로 자바스크립트에 의해 활용됩니다.
``` html
<input type="image" src="button.png" alt="이미지 버튼">
``` 
### 선택형 입력 checkbox|radio|select
체크박스의 경우 다수선택이 가능하지만 라디오버튼의 경우 하나의 그룹을 형성하고, 그 중 하나만 선택되는 폼 요소 입니다. 

`<input`
- type = "checkbox | radio"
- name = "요소 이름"
- value = "폼 요소가 선택된 상태일 때, 웹서버에 전송되는 값"
- checked : 초기에 선택 상태로 출력

체크박스와 달리 라디오 버튼은 같은 name 속성 값을 가진 라디오 버튼 중 하나만 선택 가능하게 합니다.
``` html
<form>
    <input type="radio" name="china" value="1"> 짜장면
    <input type="radio" name="china" value="2" checked> 짬뽕
    <input type="radio" name="china" value="3"> 탕수육
</form>
``` 
![radio_exp](/assets/images/프론트엔드사진/radio.png)

**콤보박스 select 태그**
`<select`
- name = "요소 이름"
- size = "창에 보일 수 있는 최대 항목 개수"
- multiple : 다수 항목 선택 가능 속성 >
여러 개의 option 태그로 선택 항목 만들기

`</select>` 
<br>
`<option` 
- value = "항목 선택되었을 때 웹 서버에 전송되는 값"
- selected >
항목 문자열

`</option>`

### 폼 요소의 캡션 만들기 label
`<label>` 태그를 사용하면 캡션과 폼 요소를 한 단위로 묶어 사용할 수 있습니다. label 태그로 문자열과 이미지를 선택형 요소와 캡션으로 만들게 되면 글자나 이미지를 클릭해도 라디오 버튼을 선택할 수 있게 됩니다.
``` html
<label>
    <input type="radio" name="china" value="1">
    짜장면 <img src="jajang.png">
</label>
``` 
### 시간 정보 입력
![timeinfo](/assets/images/프론트엔드사진/datetime.png)

### 입력할 정보의 형식 보여주기 placeholder
`<input>` 태그의 `placeholder` 속성으로 창에 입력할 형식을 사용자에게 알려줄 수 있습니다. 또한 input 태그 내의 type 속성의 속성 값에 따라 submit 버튼이 클릭되면 형식 검사가 수행되고 입력 텍스트가 형식에 맞지 않으면 오류 메시지를 출력하고 전송은 하지 않게 됩니다.

``` html
<form>
    이메일 주소:
    <input type="email" placeholder="id@host">
    <input type="submit" value="전송">
</form>
```
초기 화면 - placeholder가 입력 형식을 보여주고 있습니다.
![placeholder](/assets/images/프론트엔드사진/placeholder.png)

잘못된 형식 입력후
![placeholdererror](/assets/images/프론트엔드사진/placeholdererror.png)

`<input type="url">` : URL 입력 받는 태그
`<input type="search">` : 검색어 입력 받는 태그
