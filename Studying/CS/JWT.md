---
keyword : Java, Spring
class : CS, Programming
---


### JWT
- 로그인 정보를 Server 에 저장하지 않고, Client 에 로그인정보를 JWT 로 암호화하여 저장 → JWT 통해 인증/인가

-   모든 서버에서 **동일한 Secret Key** 소유
-   Secret Key 통한 암호화 / 위조 검증 (복호화 시)
-   JWT 장/단점
    1.  장점
        
        -   동시 접속자가 많을 때 서버 측 부하 낮춤
        -   Client, Sever 가 다른 도메인을 사용할 때
            -   예) 카카오 OAuth2 로그인 시 JWT Token 사용
    2.  단점
        
        -   구현의 복잡도 증가
        -   JWT 에 담는 내용이 커질 수록 네트워크 비용 증가 (클라이언트 → 서버)
        -   기생성된 JWT 를 일부만 만료시킬 방법이 없음
        -   Secret key 유출 시 JWT 조작 가능

---

### JWT 사용 흐름 Overview
1.  Client 가 username, password 로 로그인 성공 시
    -   "로그인 정보" → JWT 로 **암호화** (Secret Key 사용)
    -   **JWT** 를 Client 응답에 전달
	-  Client 에서 JWT 저장 (쿠키, Local storage 등)

2.  Client 에서 JWT 통해 인증방법
    -  JWT 를 API 요청 시마다 Header 에 포함
        
        예) HTTP Headers
    -   Server
	    -  Client 가 전달한 **JWT 위조 여부 검증** (Secret Key 사용)
	    -   JWT 유효기간이 지나지 않았는지 검증
	    -   검증 성공시,
	        -   JWT → "로그인 정보" (**UserDetailsImpl**) 만들어 사용
	            
	            ex) **GET /api/products** : JWT 보낸 사용자의 관심상품 목록 조회


---

### JWT 구조
    -   JWT 는 누구나 평문으로 복호화 가능
        
    -   하지만 Secret Key 가 없으면 JWT 수정 불가능
        
        → 결국 JWT 는 **Read only 데이터**



1.  **JWTAuthFilter** : API 요청 Header 에 전달되는 JWT 유효성 인증
    1.  모든 API 에 대해 JWTAuthFilter 가 JWT 확인
    2.  로그인 전 허용이 필요한 API 는 예외처리 필요 ⇒ FilterSkipMatcher
        1.  ex) 로그인 폼 페이지, 로그인 처리, css 파일 등


2.  **FormLoginFilter** : 회원 폼 로그인 요청 시 username / password 인증

-   인증 성공 시 응답 (Response) 에 JWT 포함
    
    : JWT 전달방법은 개발자가 정함
    
    예) 응답 **Header** 에 아래 **형태**로 JWT 전달


---

##### JWT 로그인 적용인증 처리 (Filter)
    
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


##### 인증 처리 (Provider)
    
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


##### 인증된 사용자 정보 처리
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