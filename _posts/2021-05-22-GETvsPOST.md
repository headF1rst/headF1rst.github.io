---
toc : true
title : "[Network] GET method VS POST method"
category : 
    - 컴퓨터네트워크
---

client가 서버에 정보를 보내는 방법에는 크게 GET과 POST, 이 두가지 방법이 있습니다. 둘다 데이터를 서버에 보내준다는 공통점이 있지만 다른 장단점도 존재하기 때문에 상황에 맞는 적절한 method를 선택하여야 합니다.

### GET method로 데이터 전송

Get 메소드는 client의 데이터를 URL 뒤에 붙여서 보냅니다. 이때 데이터는 key와 value의 쌍으로 구성되며 HTTP패킷의 헤더에 포함되어 서버에 요청하게 됩니다.

> GET/RegisterStudent.asp?user=value1&pass=value2  

위와 같은 형태로 데이터가 보내지게 되는데, URL뒤의 `"?"`를 통해 URL의 끝과 데이터 표현의 시작점을 알립니다. 여기서 key는 "user"와 "pass"이며 각각 value1, value2와 쌍으로 표현되어 있는것을 보실수 있습니다.

데이터를 URL에 붙이기 때문에 Body는 아래와 같이 빈상태로 보내지게 됩니다. 따라서 헤더 내용에 Body내용을 설명하는 Content-Type 헤더 필드 또한 자연스럽게 생략되는 것입니다.

![GETmethod_Layout](/assets/images/ComputerNetwork/GETmethod_Layout.png)

URL형태로 표현되기 때문에 특정 패키지를 다른사람 에게 접속하게 할 수 있습니다. 반면 URL에 데이터가 표시된다는 특징 때문에 보내는 데이터를 client측에서 전부 볼 수 있고, 이는 보안 문제를 야기하기도 합니다.

간단한 데이터를 넣도록 설계되어, 데이터를 보내는 양에 한계가 있으며 데이터의 저장이 간편하다는 특징이 있습니다.

### POST method로 데이터 전송

POST 메소드는 GET과 달리 데이터를 URL에 붙여서 보내지 않고 BODY에 데이터를 넣어서 보내게 됩니다. 

> POST/RegisterStudent.asp HTTP/1.1  
Host: www.headf1rst.github.io 
user=value1&pass=value2  

따라서 헤더필드중 BODY의 데이터를 설명하는 Content-Type이라는 헤더필드가 포함되며 어떤 컨텐츠 타입인지 명시해 줘야 합니다.

여러 Content-Type중 대표적인 세가지는 다음과 같습니다.

1. >application/x-www-form-urlencoded

구분자 &를 사용하며 GET과 같은 방식으로 BODY에 key, value 쌍으로 데이터를 넣습니다. $($컨텐츠를 명시해 주지 않은 경우 디폴트 값으로 사용됩니다.)
<br>

2. >text/plain

BODY에 단순 txt를 넣습니다
<br>

3. >multipart/form-data

BODY의 데이터를 binary 데이터로 넣는다는 것을 알려줍니다. 주로 파일전송에 많이 쓰이는 타입 입니다.
<br>

![POSTmethod_Layout](/assets/images/ComputerNetwork/POSTmethod_Layout.png)

### GET vs POST

- GET 메소드에서 client는 URL을 통해 value를 볼 수 있는 반면 POST에서는 value가 URL에 나타나지 않습니다.
<br>

- GET은 value의 길이에 제한이 있습니다. $($주로 255개의 문자) 반면 POST는 value가 HTTP의 BODY에 포함어 전송되기 때문에 길이에 제한이 없습니다.
<br>

- GET은 문자열 데이터 타입만 지원합니다. 반면 POST는 문자열, 바이너리, 숫자등의 다양한 데이터 타입을 지원합니다.
<br>

- GET 방식의 요청은 캐싱을 사용하여 POST 보다 빠릅니다


>참고
(https://www.guru99.com/difference-get-post-http.html)


