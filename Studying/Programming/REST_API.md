![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F4jasb%2Fbtr6ANBt8Of%2FPkRRLhzRMtMxA5xgplnDYk%2Fimg.png)

# REST API란?

- REST 는 Representational State Transfer의 약자로 웹의 장점을 최대한 활용할 수 있는 아키텍처이다.
- REST API는 웹에서 사용되는 데이터나 자원을 HTTP URI로 표현하고 HTTP 프로토콜을 통해 요청과 응답을 정의하는 방식이다.


# REST 성숙도 모델(Richardson)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb8Ejb5%2Fbtr6NkDWPsx%2Fbw7VEkdNIZ2cV8kBAA9F2K%2Fimg.png)

## Level 0 : HTTP 사용

- 웹 메커니즘을 사용하지 않고 HTTP를 원격 통신을 위한 전송 시스템으로 사용한다. RPC(Remote Precedure Call) 형태로 리소스 구분 없이 설계된 HTTP API이다.
- 단 하나의 endpoint를 사용하고, 전달되는 서로 다른 매개변수를 통해 하나의 endpoint에서 여러 동작을 하게 된다. 매개 변수를 body로 전달하기 위해 HTTP method는 POST가 된다.

### Request

```javascript
POST /api/user
{
  "function": "getUser",
  "arguments" [
    "1"
  ]
}
```

### Response

```javascript
HTTP/1.1 200 OK
{
  "result" {
    "id": "1"
    "name": "Karina",
  }
}
```

### CRUD

```javascript
CREATE : POST /api/user
READ :   POST /api/user
UPDATE : POST /api/user
DELETE : POST /api/user
```


## Level 1 : 개별 리소스 개념 도입

- 모든 요청을 단일 서비스 endpoint로 보내는 것이 아니라 개별 리소스와 통신하게 된다. 리소스별로 고유한 URI를 사용해서 식별한다.
- HTTP method는 GET과 POST만 사용하고, Response에서는 error인 경우에도 Status Code를 200으로 전달하고 있다. 또 HTTP headers에 Content-Type이나 Cache 관련 정보를 제공하지 않는다.

### Request

```javascript
POST https://api/users/create
{
  "name": "yoonyoung"
}
```

### Response

```javascript
HTTP/1.1 200 OK
{
  "result" {
    "error": "already exist member"
  }
}
```

### CRUD

```javascript
CREATE : POST /api/users/create
READ :   GET /api/users/1
UPDATE : POST /api/users/update
DELETE : POST /api/users/remove/1
```


## Level 2 : HTTP 메소드 원칙 준수

- 2단계에서는 HTTP method인 GET, POST, PUT, DELETE를 사용해 CRUD를 나타내고 메시지에 Status Code도 담겨 반환한다.
- URI에는 행위(Action)가 포함되지 않고 HTTP Method로 표현한다. GET은 매번 같은 결과를 반환하고, 헤더에 Content-Type을 제공하며 멱등성을 보장하는 GET의 경우 캐시가 적용된다.
- 현재 가장 많은 REST API가 이 단계에 해당한다고 한다.

### Request

```javascript
POST /api/users
{
  "name": "honey"
}
```

### Response

```javascript
HTTP/1.1 201 Created
Content-Type: application/json
{
  "result" {
    "id": "1",
    "name": "honey"
  }
}
```

### CRUD

```javascript
CREATE : POST /api/users
READ :   GET /api/users/1
UPDATE : PUT /api/users/1
DELETE : DELETE /api/users/1
```


## Level 3 : HATEOAS 원칙 준수

### Hypermedia As The Engine of Application State

> Hypermedia(링크)를 통해서 다음 가능한 행동(action)에 대한 정보를 응답 본문에 넣어주어야 한다.

- 예를 들어서 게시글을 조회하는 URI가 있다고 가정을 하고
### Request

```javascript
GET https://api/post/12
```

- 해당 글을 조회한 사용자는 아래와 같은 행동들을 할 수 있다.

	-   댓글 달기
	-   목록으로 돌아가기

- 이런 행동들이 바로 상태 전이가 가능한 것들인데, 이것들을 응답 본문에 **Hypermedia(링크)** 를 통해 넣어주어야 한다는 것이다.

### Response

```javascript
HTTP/1.1 200 OK
Content-Type: application/json
{
 "result" {
    "id": "12",
    "title": "고양이 사료 추천 좀 해주세요!",
    "content": "기호성 좋고 성분 좋은 거 추천 좀",
    "_links": {
      "self" : {
        "href" : "https:/api/post/12"
      },
      "comment" : {
        "href" : "https:/api/post/12/comment/1"
      },
      "list" : {
        "href" : "https:/api/posts/"
      },
  }
}
```


---

### 관련 자료

- [REST API 성숙도 모델(Maturity Model)](https://velog.io/@younge/REST-API-%EC%84%B1%EC%88%99%EB%8F%84-%EB%AA%A8%EB%8D%B8-Maturity-Model-eqqyjqff)
- [HATEOAS를 모르면 당신이 알고 있는 REST API는 REST API가 아니라고 장담할게요.](https://wonit.tistory.com/454)