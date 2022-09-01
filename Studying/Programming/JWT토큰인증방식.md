---
keyword : Spring, Security
class : CS
---


# JWT 토큰 인증방식


## **JWT의 등장배경**

JWT가 무엇이고 어떤 형태로 사용되는지도 중요하지만, 먼저 그 배경을 알아야 어떤 장점과 특징을 가지고 있는지 제대로 이해할 수 있다. JWT는 어떻게 등장하게 되었을까?

### HTTP의 특징 : contactless, stateless

HTTP는 contactless(클라이언트의 요청이 처리되면 연결이 끊어짐), stateless(서버가 클라이언트의 이전 상태를 저장하지 않음)라는 특징을 가지고 있어 인증이 필요한 페이지에 접근할 경우 서버는 클라이언트가 누구인지 매번 확인해야했다. 예를 들어 쇼핑몰에서 로그인을 하고 마음에 드는 제품을 장바구니에 담을 때마다 다시 로그인을 하며 내가 회원이라는 걸 알려줘야 했다는 것. 상상만해도 번거롭다.

### 서버기반 인증

인증이 필요한 페이지에서 매번 로그인을 해야하는 이러한 불편을 보완하고자 가장 먼저 **쿠키와 세션을 활용한 서버기반 인증 방식**이 등장했다.


##### cookie (쿠키)

![](https://k.kakaocdn.net/dn/cuj5lk/btrD1sqc6rh/x2hfjkfV6z2C5HEDOlgxC1/img.png)

아마 쿠키라 한다면 위 화면과 함께 티켓팅, 수강신청 꿀팁 중 하나인 '_쿠키를 삭제해라_' 를 떠올리는 사람들이 대부분이지 않을까? 지금껏 아무 생각 없이 쿠키를 삭제하는 작업을 했었지만, 내가 삭제한 쿠키가 정확히 뭘 의미하는지는 몰랐다.

그래서 한 마디로 정리해보자면, 쿠키는 **client가 웹사이트에 접속할 때 그 사이트의 서버를 통해 브라우저에 설치되는 작은 기록 정보 파일**이다. 여기서 '작은'이라 칭한 이유는 기록할 수 있는 데이터 용량이 작고 귀여운 4kb이기 때문이다.

**쿠키를 활용한 인증 프로세스**

![](https://k.kakaocdn.net/dn/bMGdyg/btrEQTBxxEo/y7TQM7n6SGrLjkpaVy3K9K/img.png)

1.  Client는 Server에 어떠한 요청을 한다. (ex. 로그인 요청)
2.  Server는 요청에 대한 응답의 Header의 set-cookie에 Client에 저장하고 싶은 정보를 담아 보낸다.
3.  Client는 요청을 보낼 때마다 Header의 cookie에 전달받았던 정보를 함께 보낸다.
4.  Server는 전달받은 cookie의 정보로 Client를 식별한다. 

**쿠키의 단점**

-   보안 취약성 : Header를 통해 그대로 전달되므로 유출될 위험이 높다.
-   용량 제한 : 4kb라는 작고 귀여운 용량으로 제한된다.
-   브라우저간 공유 불가 : 서로 다른 각 웹 브라우저마다 지원하는 방식이 다르기 때문에 공유가 되지 않는다. 

##### session

세션은 Client의 정보를 Client쪽이 아닌 Server쪽에 저장하고 관리함으로써 보안이 취약한 cookie의 단점을 보완해준다.

**세션을 활용한 인증 프로세스**

![](https://k.kakaocdn.net/dn/NtWRc/btrEV0nkSUO/vwrgrw5LljmSVaxp2TdbKK/img.png)

1. Client는 Server에 어떠한 요청을 한다. (ex. 로그인 요청)

2. Server는 메모리에 session Id를 기준으로 session을 생성하고, 정보를 저장한다.

3. Server는 Client의 요청에 대한 응답으로 session Id를 전달한다.

4. Client는 Server에 요청을 할 때마다 cookie에 session Id를 함께 전달한다.

5. Server는 전달받은 cookie의 session 정보와 이전에 발급해 전달했던 session 정보를 비교한다.

6. Server는 인가 성공/실패 여부를 Client에게 전달한다.

**세션의 단점**

-   서버 부하 : Server에 세션을 저장하는 저장소가 필요하고, 저장소에 데이터 정보 조회 요청을 계속 보내야하므로 서버에 부담이 된다.
-   stateful : Server가 정보를 계속 기억하므로 HTTP의 특징인 stateless에 위배된다.


### 토큰 기반 인증, JWT의 등장

**토큰 기반 인증**

앞서 언급한 서버의 부담, stateless 위배라는 서버기반 인증의 약점을 보완하기위해 토큰 기반 인증이 등장했다. 토큰 기반 인증은 (1) 인증받은 Client에게 Server가 토큰을 부여하고 (2) 인증이 필요한 요청의 HTTP Header에 이 토큰을 담아 보내면 (3) 토큰을 기준으로 Server가 Client를 식별하는 방식이다.

**JWT (JSON Web Token)**

인증에 필요한 정보들을 암호화시킨 토큰을 뜻한다.

JWT 기반 인증 방식이란 Client가 Server에게 요청을 보낼 때 HTTP의 Header에 이전에 Server로부터 발급받은 JWT를 담아 보내고, 이를 기반으로 Server가 Client를 식별하는 방식을 뜻한다.

## **JWT의 구조**

```
XXXXXX.YYYYYY.ZZZZZZ
// Header(헤더).Payload(내용).Signature(서명)
```

JWT는 ' . '로 구분된 세 문자열인 Header, Payload, Signature로 이루어져있다.

각각의 구조를 더 딥하게 알아보자.

### Header

signature를 암호화하는 알고리즘과 토큰의 유형을 포함한다.

```
// 디코딩된 형태
{
"alg" : ... , //서명 암호화 알고리즘
"typ" : "JWT", //토큰 유형
}
```

### Payload

서버와 클라이언트가 주고받는 시스템 속에서 실제로 사용될 정보  
* claim : key-value 형식의 정보 한 쌍

-   Registered claims : 미리 정의된 클레임 _ex. iss(발행자), exp(만료시간), iat(발생시간) 등_
-   Public claims : 공개용 정보 전달을 위한 클레임 (사용자가 정의가능))
-   Private claims : 당사자들간 정보 공유용 사용자 지정 클레임

### Signature 

header의 인코딩 값 + payload의 인코딩 값 + 서버의 key값 을 header에서 정의한 알고리즘으로 암호화한 것.  
이 때 서버의 key값이 유출되지 않는 이상 복호화할 수 없다.

## **JWT를 이용한 인증 Process**

![](https://k.kakaocdn.net/dn/bIAswX/btrE1YVPHPe/4ze7eMN0WsgPZuvU6FBX2k/img.png)

1.  Client가 Server에 요청을 보낸다. (ex. 로그인 인증 요청)
2.  Server는 Header, Payload, Signature를 정의하고 각각 암호화해 JWT를 생성한다.
3.  Server는 생성한 JWT를 cookie에 담아 Client에게 발급한다.
4.  Client는 JWT를 저장하고, 이후 인증이 필요한 요청마다 Authorization Header에 Access token을 담아 보냄
5.  Server는 발행했던 token과 Client로부터 받은 token의 일치여부를 확인한다 (인증).
6.  token이 서로 일치하면 payload의 정보를 Client에게 전달한다.
7.  Access token이 만료될 경우, Client는 refresh token을 Server에 보낸다.
8.  Server는 새로운 Access token을 발급해 Client에 전달한다.

## **JWT의 장점**

-   Header와 Payload로 signature를 생성하기 때문에 데이터가 위,변조되었을 때 더 쉽게 판별할 수 있다.
-   Session과 달리  
    - 별도 저장소가 불필요하다.  
    - 서버를 stateless로 유지할 수 있다.  
    - 정보를 자체적으로 보유할 수 있어 요청할 때마다 DB조회를 하지않아도 된다.  
    - 모바일 환경에서도 잘 동작한다.

## **JWT의 단점**

-   token의 길이가 길어 인증 요청이 많아질수록 네트워크 부하 위험이 있다. (DB 성능면에선 이득이지만)
-   token은 탈취당할 경우 대처하기가 어렵다.
-   token의 만료시간을 설정해두었을 경우, 그 전에 강제로 만료시키기 어렵다.
-   payload의 내용은 암호화가 되지않아, Server와 Client간에 나누는 내용에 중요한 정보 (ex. 고객 개인정보)를 담을 수는 없다.
    
    -   JWT 사용 흐름 Overview
        
        1.  Client 가 username, password 로 로그인 성공 시
            1.  "로그인 정보" → JWT 로 **암호화** (Secret Key 사용)
                
            2.  **JWT** 를 Client 응답에 전달
                
            3.  Client 에서 JWT 저장 (쿠키, Local storage 등)
                
            4.  Client 에서 JWT 통해 인증방법
                
                1.  JWT 를 API 요청 시마다 Header 에 포함
                    
                    예) HTTP Headers
                    
                    ```json
                    Content-Type: application/json
                    **Authorization: Bearer** **<JWT>
                    ...**
                    ```
                    
                2.  Server
                    
                    1.  Client 가 전달한 **JWT 위조 여부 검증** (Secret Key 사용)
                    2.  JWT 유효기간이 지나지 않았는지 검증
                    3.  검증 성공시,
                        1.  JWT → "로그인 정보" (**UserDetailsImpl**) 만들어 사용
                            
                            ex) **GET /api/products** : JWT 보낸 사용자의 관심상품 목록 조회
                            
    -   JWT 구조
        
        -   JWT 는 누구나 평문으로 복호화 가능
            
        -   하지만 Secret Key 가 없으면 JWT 수정 불가능
            
            → 결국 JWT 는 **Read only 데이터**
        
        1.  Header
            
            ```json
            {
              "alg": "HS256",
              "typ": "JWT"
            }
            ```
            
        2.  Payload
            
            ```json
            {
              "sub": "1234567890",
              "username": "제이홉",
              "admin": true
            }
            ```
            
        3.  Signature
            
            ```json
            HMACSHA256(
              base64UrlEncode(header) + "." +
              base64UrlEncode(payload),
              secret)
            ```
            

1.  **JWTAuthFilter** : API 요청 Header 에 전달되는 JWT 유효성 인증
    1.  모든 API 에 대해 JWTAuthFilter 가 JWT 확인
    2.  로그인 전 허용이 필요한 API 는 예외처리 필요 ⇒ FilterSkipMatcher
        1.  ex) 로그인 폼 페이지, 로그인 처리, css 파일 등

-   JWT 로그인 이해

![](https://k.kakaocdn.net/dn/bIAswX/btrE1YVPHPe/4ze7eMN0WsgPZuvU6FBX2k/img.png)


1.  **FormLoginFilter** : 회원 폼 로그인 요청 시 username / password 인증
    
    -   인증 성공 시 응답 (Response) 에 JWT 포함
        
        : JWT 전달방법은 개발자가 정함
        
        예) 응답 **Header** 에 아래 **형태**로 JWT 전달
        
        ```java
        **Authorization: BEARER** <JWT>
        
        ex)
        **Authorization: BEARER** eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJzcGFydGEiLCJVU0VSTkFNRSI6IuultO2DhOydtCIsIlVTRVJfUk9MRSI6IlJPTEVfVVNFUiIsIkVYUCI6MTYxODU1Mzg5OH0.9WTrWxCWx3YvaKZG14khp21fjkU1VjZV4e9VEf05Hok
        ```
![Untitled1](https://user-images.githubusercontent.com/93110733/188031146-24451c28-8d68-43ac-bbb6-8ad72649a899.png)


-   JWT 로그인 적용인증 처리 (Filter)
    
    -   Filter 는 Client 의 API 요청이 Controller 에 전달되기 전, 사전처리를 하는 영역
    -   즉, Controller 에 도달하기 전에 인증 처리를 하기 위해 사용
    
    1.  **FormLoginFilter** : 회원 폼 로그인 요청 시 username / password 인증
        
        1.  **POST "/user/login"** API 에 대해서만 동작 필요
            1.  "GET /user/login" 가 처리되지 않게 하기 위해 API 주소 변경
                1.  "GET /user/login" → "GET /user/loginView"
        2.  Client 로부터 username, password 를 전달받아 인증 수행
        3.  인증 성공 시
            1.  FormLoginSuccessHandler 통해 JWT 생성
            2.  이후 Client 에서는 모든 API 응답 Header 에 JWT 를 포함하여 인증
    2.  **JWTAuthFilter** : API 요청 Header 에 전달되는 JWT 유효성 인증
        
-   인증 처리 (Provider)
    
    -   Filter 가 인증에 필요한 정보를 적합한 클래스 형태로 만들어 Spring Security 에 인증 요청을 함
    -   Spring Security 는 Filter 가 요청한 인증 처리를 할 수 있는 Provider 를 찾고, 실제 인증처리는 Provider 에 의해 진행됨
        -   인증처리 가능 여부 판단기준:
            
            supports 함수 통해 "인증정보의 클래스 타입"을 보고 판단
            
            ```java
            @Override
            public boolean supports(Class<?> authentication) {
                return authentication.equals(UsernamePasswordAuthenticationToken.class);
            }
            ```
            
    
    1.  FormLoginAuthProvider
        
        -   Client 에서 전달한 ID/PW 가 DB 의 ID/PW 와 일치하는지 인증
            
            ```java
            @Override
            public Authentication authenticate(Authentication authentication) throws AuthenticationException {
                UsernamePasswordAuthenticationToken token = (UsernamePasswordAuthenticationToken) authentication;
                // FormLoginFilter 에서 생성된 토큰으로부터 아이디와 비밀번호를 조회함
                String username = token.getName();
                String password = (String) token.getCredentials();
            
                // UserDetailsService 를 통해 DB에서 username 으로 사용자 조회
                UserDetailsImpl userDetails = (UserDetailsImpl) userDetailsService.loadUserByUsername(username);
                if (!passwordEncoder.matches(password, userDetails.getPassword())) {
                    throw new BadCredentialsException(userDetails.getUsername() + "Invalid password");
                }
            
                return new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());
            }
            ```
            
        -   인증 성공 시 → FormLoginSuccessHandler 에서 응답 Header 에 JWT 포함시킴
            
            ```java
            public class FormLoginSuccessHandler extends SavedRequestAwareAuthenticationSuccessHandler {
                public static final String AUTH_HEADER = "Authorization";
                public static final String TOKEN_TYPE = "BEARER";
            
                @Override
                public void onAuthenticationSuccess(final HttpServletRequest request, final HttpServletResponse response,
                                                    final Authentication authentication) {
                    final UserDetailsImpl userDetails = ((UserDetailsImpl) authentication.getPrincipal());
                    // Token 생성
                    final String token = JwtTokenUtils.generateJwtToken(userDetails);
                    response.addHeader(AUTH_HEADER, TOKEN_TYPE + " " + token);
                }
            
            }
            ```
            
    2.  JWTAuthProvider
        
        -   Client 의 Header 로 전달된 JWT 가 유효한지 검증
            
            ```java
            @Override
            public Authentication authenticate(Authentication authentication)
                    throws AuthenticationException {
                String token = (String) authentication.getPrincipal();
                String username = jwtDecoder.decodeUsername(token);
            
                // TODO: API 사용시마다 매번 User DB 조회 필요
                //  -> 해결을 위해서는 UserDetailsImpl 에 User 객체를 저장하지 않도록 수정
                //  ex) UserDetailsImpl 에 userId, username, role 만 저장
                //    -> JWT 에 userId, username, role 정보를 암호화/복호화하여 사용
                User user = userRepository.findByUsername(username)
                        .orElseThrow(() -> new UsernameNotFoundException("Can't find " + username));;
                UserDetailsImpl userDetails = new UserDetailsImpl(user);
                return new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());
            }
            ```
            
-   인증된 사용자 정보 처리
    
    -   Controller 에서 사용가능한 **인증 사용자 정보** (@AuthenticationPrincipal) 를 만듦
        
        ```java
        // 신규 상품 등록
        @PostMapping("/api/products")
        public Product createProduct(@RequestBody ProductRequestDto requestDto,
         **@AuthenticationPrincipal UserDetailsImpl userDetails**) {
            // 로그인 되어 있는 ID
            Long userId = userDetails.getUser().getId();
        
            Product product = productService.createProduct(requestDto, userId);
            // 응답 보내기
            return product;
        }
        ```
        
    -   현재 문제점) API 호출시마다 JWT 검증 후 매번 회원 DB (User) 조회 중
        
        -   원인: UserDetailsImp 에서 "User" Entity 객체를 가지고 있기 때문
        -   개선방법:
            1.  UserDetailsImpl 에 User 객체를 저장하지 않도록 수정
                
                -   **인증 사용자 정보**는 Spring Security 가 제공하는 UserDetails 인터페이스 형태만 맞춰서 구현하면됨. 멤버변수는 마음대로 수정 가능
                    
                    ex) UserDetailsImpl 에 User 객체 대신 userId, username, role 만 저장
                    
                    ```java
                    public class UserDetailsImpl implements UserDetails {
                    		// 삭제
                        private User user;
                    
                    		// 추가
                    		private Long userId;
                    	  private String username;
                    		private UserRoleEnum role;
                    ```
                    
            2.  JWT 에 userId, username, role 정보를 암호화/복호화하여 사용