# HTTP메서드

## 시작하기 전에…

### REST란?

Representational State Transger : 웹을 위한 아키텍처 스타일

리소스를 URI로 표현하고 리소스에 대한 행위를 HTTP메서드로 정의하는 방식!

## HTTP Method 종류

### HTTP 메서드란?

클라이언트와 서버 사이에서 이루어지는 요청과 응답 데이터를 전송하는 방식

서버가 주어진 리소스에 수행하길 원하는 행동, 서버거(햄버거) 해야할 동작을 지정하는 요청을 보내는 방법!

## GET

> 리소스 조회 메서드 (READ)

### GET이란 무엇인가?

- 서버에 존재하는 데이터를 요청하는 메서드
- 캐싱이 가능하다.
    - 캐싱이란? 한번 접근 후 또 요청할 시에 빠르게 접근하기 위해 레지스터에 데이터를 저장 시켜 놓는 것
- 쿼리 파라미터 없이 리소스 경로로만 단순 조회를 하는 것도 GET요청에 해당함
- GET요청으로 서버에 데이터를 전달하는 경우 **`쿼리스트링`**을 통해서 전달한다.

예시로 네이버에 역삼역 맛집을 검색하면 위와같이 url에 전달되는 것을 확인할 수 있다.

주의해야할 점 : 쿼리스트링은 클라이언트에게 전달하는 데이터정보가 무방비로 노출되기 때문에 유의해야한다.

### 조회 과정

html의 form태그는 GET과 POST를 지원한다.

아래와 같은 form이 있다고 할때 username에 KimGyungmin age에 26을 입력 후 조회 버튼을 누르면?
```html
<form action="/member" method="get">
	<input type="text" name="username"/>
	<input type="text" name="age" />
	<button type="submit">조회</button>
</form>
```

```text
GET /members?username=KimGyungmin &age=26 HTTP/1.1
Host: localhost:8080
```
이런 HTTP메시지가 생성된다.
서버는 그럼 해당 쿼리 파라미터를 기반으로 필터를 사용해 결과를 동적으로 생성 후 반환한다.

```text
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 34
{
  “username”:”KimGyungmin”,
  “age”:”26”
}
```
## POST

> 전달한 데이터 처리&생성 요청 메서드 (CREATE)

### POST란 무엇인가?

- 서버에 데이터를 생성하는 것을 요청하는 메서드다.
- 성공적으로 creation을 완료하면 201 HTTP응답을 반환한다.
- 데이터를 메시지 바디에 쿼리 파라미터 형식으로 전달한다.
    - 쿼리 파라미터는 key-value형식
    - GET과 비교했을때 데이터가 외부로 노출되지 않는다는 장점이 있음.
    - 메시지 길이의 제한이 없음!
    - 텍스트 데이터 외에도 다른 데이터를 전송할 수 있음.
    - 데이터를 GET해야하는데 애매하게 값을 JSON으로 넘기거나 할때도 POST방식이 사용된다.
- 전달된 데이터로 신규 리소스 등록, 프로세스 처리 등에 많이 사용됨(예시 : 게시판의 글 쓰기, 글 수정 등)

### 조회 과정

클라이언트는 다음과 같이 body에 정보를 담아서 JSON형태로 서버에 전송한다.
```text
POST /members HTTP/1.1
Content-Type: application/json
{
  “username”:”KimGyungmin”,
  “age”:”26”
}
```
서버에서는 위 메시지를 분석해서 로직대로 처리한다. 
그 후 아래와 같은 메시지를 클라이언트에게 응답한다.
```text
<aside>
HTTP /1.1 201 Created
Content-Type: application/json
Content-Length: 34
Location: /member/10
{
  “username”:”KimGyungmin”,
  “age”:26
}
```
신규 자원이 생성되었다는 의미로 201로 응답이 간것.
Location은 자원이 신규로 생성된 URI경로를 의미한다.

- Content-Type의 종류
  - application/json
    - text, XML, JSON 데이터 전송시 사용됨
  - multipart/form-data
    - 바이너리 데이터 전송시 사용(예:파일업로드)
    - form내용과 함께 + 다른 여러 파일을 다 담을 수 있음
  - application/x-www-form-urlencoded
    - 전송 데이터를 url encoding처리
    - url encoding은 아스키 코드에 없는 영어를 제외환 외국어, 특수문자를 표현할때 사용되는 인코딩임 가끔 URL에 한글이 들어있는게 %AC%B9%EA 이런 느낌으로 바뀌는걸 볼 수 있는데 그게 url encoding

## PUT

> 리소스를 수정하는 메서드 (UPDATE or CREATE)

서버에 데이터가 존재하면 그걸 수정하고 없다면 새로 생성하는 메서드이다.
일부만 수정하는 것은 불가능함.
```text
PUT /member/10 HTTP/1.1
Content-Type: application/json
{
  “age”:20 
}
```
위에서 만든 username : Kimgyungmin , age : 26이라는 리소스에서 age를 20으로 바꾸고 싶다고 age : 20 이 정보만 PUT으로 전송하게 되면
그냥 기존 데이터가 대체되기 때문에 username부분이 사라지게됨

## PATCH

> 리소스 일부 부분을 변경하는 메소드 (UPDATE)

```text
PATCH /member/10 HTTP/1.1
Content-Type: application/json
{
  “age”:20 
}
```
PUT과 다르게 이렇게 전송을 해도 usernaem은 남아 있고 age만 값이 변경되게 된다.
PATCH를 지원하지 않는 서버도 있는데 그럴 경우에는 대체로 POST를 이용한다.

## DELETE

> 리소스를 제거하는 메서드 (DELETE)

## 그외
### HEAD

GET과 동일하지만 Body를 return하지 않는 차이가 있음  
Resource가 필요 없고 일종의 검사 용도로 응답의 상태 코드만 확인할 때 사용함

### TRACE

이 메서드도 일종의 검사용 메서드로
서버에 도달했을 때의 최종 패킷의 요청 패킷 내용을 응답 받는다.  
위변조여부 확인에 사용된다.
![image](https://github.com/gyungmean/cs_study/assets/70059000/79d05cce-074b-41d2-a2ae-4d9bf030dcb2)

### OPTIONS

해당 URI에 대해 서버가 허용하는 메서드를 확인할 때 사용하는 메서드  
클라이언트 : (OPTIONS 요청)  
서버 : 나 GET, POST, PUT, OPTIONS만 사용가능하긔ㅇㅇ  
이런식의 대화가 이루어지는 느낌~

---
# :pencil: 퀴즈
> GET메서드를 사용할 때 사용자가 URL을 조작해서 접근하면 안되는 정보에 접근하려고 합니다. 이걸 방지하려면 어떻게 하는 것이 좋을까요?