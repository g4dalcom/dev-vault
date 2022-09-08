---
keyword : Spring, JWT, security
class : Programming
---


# Refresh token이 왜 필요한가

Access Token만을 통한 인증 방식의 문제는만일 제 3자에게 탈취당할 경우 보안에 취약하다는 점이다.Access Token은 발급된 이후, 서버에 저장되지 않고 토큰 자체로 검증을 하며 사용자 권한을 인증하기 떄문에, Access Token이 탈취되면 토큰이 만료되기 전 까지, 토큰을 획득한 사람은 누구나 권한 접근이 가능해 지기 때문이다.JWT는 발급한 후 삭제가 불가능하기 때문에, 접근에 관여하는 토큰에 유효시간을 부여하는 식으로 탈취 문제에 대해 대응을 하여야 한다.이처럼 토큰 유효기간을 짧게하면 토큰 남용을 방지하는 것이 해결책이 될 수 있지만, 유효기간이 짧은 Token의 경우 그만큼 사용자는 로그인을 자주 해서 새롭게 Token을 발급받아야 하므로 불편하다는 단점이 있다. 그렇다고 무턱대고 유효기간을 늘리자면, 토큰을 탈취당했을 때 보안에 더 취약해지게 된다.

이때 “그러면 유효기간을 짧게 하면서  좋은 방법이 있지는 않을까?”라는 질문의 답이 바로Refresh Token이다.

이름이 다르지만 형태 자체는 Refresh Token은 Access Token과 똑같은 JWT다.단지 Access Token은접근에 관여하는 토큰이고, Refresh Token은재발급에 관여하는 토큰 이므로 행하는 역할이 다르다고 보면 된다.

예를 들면 처음에 로그인을 했을 때, 서버는 로그인을 성공시키면서 클라이언트에게Access Token과 Refresh Token을 동시에 발급한다.

서버는 데이터베이스에 Refresh Token을 저장하고, 클라이언트는 Access Token과 Refresh Token을 쿠키, 세션 혹은 웹스토리지에 저장하고 요청이 있을때마다 이 둘을 헤더에 담아서 보낸다.이 Refresh Token은 긴 유효기간을 가지면서, Access Token이 만료됐을 때 새로 재발급해주는 열쇠가 된다.

따라서 만일 만료된 Access Token을 서버에 보내면, 서버는 같이 보내진 Refresh Token을  DB에 있는 것과 비교해서 일치하면 다시 Access Token을 재발급하는 간단한 원리이다.그리고 사용자가로그아웃을 하면 저장소에서 Refresh Token을 삭제하여 사용이 불가능하도록 하고 새로 로그인하면 서버에서 다시 재발급해서 DB에 저장한다.

<aside> 💡 Tip : 사용 예를 간단히 들어보자. Refresh Token의 유효기간은 2주, Access Token의 유효기간은 1시간이라 가정 하겠다.

사용자는 API 요청을 하다가 1시간이 지나게 되면, 가지고 있는 Access Token은 만료 되게 된다. 그러면 Refresh Token의 유효기간 전까지는 Access Token을 새롭게 발급받을 수 있게 된다. 즉, Refresh Token 접근에 대한 권한을 주는 것이 아니라 Access Token 재발급에만 관여하는 것이다.

만일 Refresh Token의 유효기간(2주)이 만료됐다면, 사용자는 새로 로그인해야 한다. Refresh Token도 탈취될 가능성이 있기 때문에 이 역시 적절한 유효기간 설정이 필요하다 (보통 2주로 많이 잡는 편이다)

</aside>

# Access / Refresh Token 재발급 원리

1. 기본적으로 로그인 같은 과정을 하면 Access Token과 Refresh Token을 모두 발급한다.이때, Refresh Token만 서버측의 DB에 저장하며, Refresh Token과 Access Token을 쿠키 혹은 웹스토리지에 저장한다.

2. 사용자가 인증이 필요한 API에 접근하고자 하면, 가장 먼저 토큰을 검사한다.이때, 토큰을 검사함과 동시에 각 경우에 대해서 토큰의 유효기간을 확인하여 재발급 여부를 결정한다.

🔎 case1 : access token과 refresh token 모두가 만료된 경우→ 에러 발생 (재 로그인하여 둘다 새로 발급)

🔎 case2 : access token은 만료됐지만, refresh token은 유효한 경우→  refresh token을 검증하여 access token 재발급

🔎 case3 : access token은 유효하지만, refresh token은 만료된 경우→  access token을 검증하여 refresh token 재발급

🔎 case4 : accesss token과 refresh token 모두가 유효한 경우→ 정상 처리

<aside> 💡 Tip

[refresh token을 검증하여 access token 재발급] 클라이언트(쿠키, 웹스토리지)에 저장되어있는 refresh token과 서버 DB에 저장되어있는 refresh token 일치성을 확인한 뒤 access token 재발급한다.

[access token을 검증하여 refresh token 재발급] access token이 유효하다라는 것은 이미 인증된 것과 마찬가지니 바로 refresh token 재발급한다.

</aside>

3.  로그아웃을 하면 Access Token과 Refresh Token을 모두 만료시킨다.

# Refresh Token 인증 과정

1.  사용자가 ID , PW를 통해 로그인.
    
2.  서버에서는 회원 DB에서 값을 비교
    
3.  로그인이 완료되면 Access Token(JWT), Refresh Token을 발급한다. 이때 회원DB에도 Refresh Token을 저장해둔다.
    
4.  사용자는 Refresh Token은 안전한 저장소에 저장 후, Access Token을 헤더에 실어 요청을 보낸다.
    
5.  Access Token을 검증하여 이에 맞는 데이터를 보낸다.
    
6.  시간이 지나 Access Token이 만료됐다.
    
7.  사용자는 이전과 동일하게 Access Token을 헤더에 실어 요청을 보낸다.
    
8.  서버는 Access Token이 만료됨을 확인하고 권한없음을 신호로 보낸다.
    

<aside> 💡 Tip Access Token 만료가 될 때마다 계속 과정 7~8을 거칠 필요는 없다. 사용자(프론트엔드)에서 Access Token의 Payload를 통해 유효기간을 알 수 있다. 따라서 프론트엔드 단에서 API 요청 전에 토큰이 만료됐다면 곧바로 재발급 요청을 할 수도 있다.

</aside>

9.  사용자는 Refresh Token과 Access Token을 함께 서버로 보낸다.
    
10.  서버는 받은 Access Token이 조작되지 않았는지 확인한후, Refresh Token과 사용자의 DB에 저장되어 있던 Refresh Token을 비교한다. Token이 동일하고 유효기간도 지나지 않았다면 새로운 Access Token을 발급해준다.
    
11.  서버는 새로운 Access Token을 헤더에 실어 다시 API 요청 응답을 진행한다.
    

출처:

[](https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-Access-Token-Refresh-Token-%EC%9B%90%EB%A6%AC-feat-JWT)[https://inpa.tistory.com/entry/WEB-📚-Access-Token-Refresh-Token-원리-feat-JWT](https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-Access-Token-Refresh-Token-%EC%9B%90%EB%A6%AC-feat-JWT)

[👨‍💻 Dev Scroll:티스토리]