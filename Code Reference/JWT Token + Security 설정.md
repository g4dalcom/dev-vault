## configuration

- JwtSecurityConfiguration

```java
//jwt 시큐리티 설정하는 곳! 해줘야 필터가 제 순서대로 돌아감

@RequiredArgsConstructor
public class JwtSecurityConfiguration extends SecurityConfigurerAdapter<DefaultSecurityFilterChain, HttpSecurity> {

	private final String SECRET_KEY;
	private final TokenProvider tokenProvider;
	private final UserDetailsServiceImpl userDetailsService;

	//회원의 이름, 비밀번호 필터 실행전에 jwt custom 필터 먼저 실행
	@Override
	public void configure(HttpSecurity httpSecurity) {
	
		JwtFilter customJWTFilter = 
			new JwtFilter(SECRET_KEY, tokenProvider, userDetailsService);
		
		httpSecurity.addFilterBefore(customJWTFilter, 
			UsernamePasswordAuthenticationFilter.class);
	
	} 

} 
```

- SecurityConfiguration
```java

//시큐리티 사용 (권한 부여 -> 회원만 글 cud를 위해 접근 경로 설정

@Configuration
@RequiredArgsConstructor
@EnableWebSecurity //SpringSecurityFilterChain이 자동으로 포함
@ConditionalOnDefaultWebSecurity
@ConditionalOnWebApplication(type = ConditionalOnWebApplication.Type.SERVLET)

public class SecurityConfiguration {

	@Value("${jwt.secret}")
	String SECRET_KEY; //시크릿키에 임의로 지정한 키 값 받아오기
	
	private final TokenProvider tokenProvider;
	private final UserDetailsServiceImpl userDetailsService;
	private final AuthenticationEntryPointException 
									authenticationEntryPointException;
	private final AccessDeniedHandlerException 
									accessDeniedHandlerException;


	//password 암호화 해주는 인코더 설정
	@Bean
	public PasswordEncoder passwordEncoder(){
	
		return new BCryptPasswordEncoder();
	
	} 

	// h2-console 사용에 대한 허용 (CSRF, FrameOptions 무시)
	@Bean
	public WebSecurityCustomizer webSecurityCustomizer(){
	
	return (web) -> web.ignoring()
	.antMatchers("/h2-console/**");
	
	} 

	@Bean
	@Order(SecurityProperties.BASIC_AUTH_ORDER)
	public SecurityFilterChain filterChain(HttpSecurity http) throws Exception{
	http.cors(); //CORS는 한 도메인 또는 Origin의 웹 페이지가 다른 도메인 (도메인 간 요청)을 가진 리소스에 액세스 할 수 있게하는 보안 메커니즘
	
	//서버와 클라이언트가 정해진 헤더를 통해 서로 요청이나 응답에 반응할지 결정하는 방식
		http.csrf().disable()
		//csrf-> 정상적 사용자가 의도치 않은 위조요청 보내는 것 -> 현재는 x, disable
	
	.exceptionHandling()
	.authenticationEntryPoint(authenticationEntryPointException)
	.accessDeniedHandler(accessDeniedHandlerException)
		//handler, 에러 처리 설정
	
	.and()
	.sessionManagement()
	.sessionCreationPolicy(SessionCreationPolicy.STATELESS)
		//스프링 시큐리티가 생성하지도 않고 기존 것을 사용하지도 않음 ->JWT 같은 토큰방식을 쓸때 사용하는 설정
	
	.and()
	.authorizeRequests()
		//시큐리티 처리에 HttpServletRequest를 이용
	
	.antMatchers("/api/users/**").permitAll()
	.antMatchers("/api/posts/**").permitAll()
	.antMatchers("/api/comments/**").permitAll()
	.anyRequest().authenticated()
		//위 네 개 빼고 다 권한 받아야 이용가능 루트
	
	.and()
	
	.apply(new JwtSecurityConfiguration(SECRET_KEY, tokenProvider, userDetailsService ));
	
	return http.build();
	
	}

}

```

- WebMvcConfig
```java

	@Configuration
	public class WebMvcConfig implements WebMvcConfigurer {
	
	private final long MAX_AGE_SECS = 3600;
	
	@Override
	public void addCorsMappings(CorsRegistry registry) {
	
		registry.addMapping("/**")
		//origin이 localhost:3000
				.allowedOrigins("http://localhost:3000") 
		//허용하는 메소드
				.allowedMethods("GET", "POST", "PUT", "DELETE", "PATCH", "OPTIONS") 
		//헤더
				.allowedHeaders("*") 
		//인증 정보
				.allowCredentials(true) 
				.maxAge(MAX_AGE_SECS)
		//cors환경에서 헤더 값 제대로 반환을 위해 헤더 정보 출력
				.exposedHeaders("*"); 
		
		//서버가 요청에 대한 응답으로 브라우저에서 실행되는 스크립트에 제공되어야하는 표시
	
	}

}
	
```

- Cors 설정 또다른 버전(SecurityConfiguration 안에 넣어도 됨)
```java
@Bean  
public CorsConfigurationSource corsConfigurationSource() {  
		CorsConfiguration configuration 
				= new CorsConfiguration();  
	  
		configuration.addAllowedOriginPattern("*");  
		configuration.addAllowedOrigin("http://localhost:3000");  
		configuration.addAllowedHeader("*");  
		configuration.addAllowedMethod("*");  
		configuration.addExposedHeader("Authorization");  
		configuration.addExposedHeader("RefreshToken");  
		configuration.setAllowCredentials(true);  
	  
		UrlBasedCorsConfigurationSource source 
				= new UrlBasedCorsConfigurationSource();  
		source.registerCorsConfiguration("/api/**", configuration);  
		return source;  
}
```



## domain
- RefreshToken
```java
@Getter
@NoArgsConstructor
@AllArgsConstructor
@Builder
@Entity
public class RefreshToken extends Timestamped {

	@Id
	@Column(nullable = false)
	private Long id;
	
	@JoinColumn(name = "users_id", nullable = false)
	@OneToOne(fetch = FetchType.LAZY)
	private Users users;
	
	@Column(nullable = false)
	private String keyValue; //refresh token 값

}
```

- UserDetailsImpl
```java

//DB에 접근해 사용자(회원가입된 유저)의 정보 가져옴
@Data
@NoArgsConstructor
@AllArgsConstructor
public class UserDetailsImpl implements UserDetails {

	private Users users;
	
	@Override
	public Collection<? extends GrantedAuthority> getAuthorities(){
	//사용자에게 부여된 권한 반환
	SimpleGrantedAuthority authority = new SimpleGrantedAuthority(Authority.ROLE_USER.toString());
	
	//회원가입한 유저를 ROLE_USER의 권한을 부여함 = auth 부분 가능
	Collection<GrantedAuthority> authorities = new ArrayList<>();
	authorities.add(authority);
	
	return authorities;
	
	}
	
	//사용자를 인증하는데 사용된 암호 반환
	@Override
	public String getPassword(){
		return users.getPassword();
	
	} 
	
	//사용자를 인증하는데 사용된 사용자 이름 반환
	@Override
	public String getUsername(){
	
		return users.getName();
	
	} 
	
	//사용자의 계정이 만료되었는지 여부, 만료된 계정은 인증 불가
	@Override
	public boolean isAccountNonExpired() {
	
		return true;
	
	}
	
	//사용자가 잠겨 있는지 여부, 잠긴 사용자는 인증 불가
	@Override
	public boolean isAccountNonLocked(){ return true; }
	
	//사용자의 자격 증명(암호)이 만료되었는지 여부를 나타냄, 만료된 자격증명은 인증 방해
	@Override
	public boolean isCredentialsNonExpired(){ return true; }
	
	//사용자가 활성화되어있는지 여부, 비활성된 사용자 인증 불가
	@Override
	public boolean isEnabled(){ return true; }
	
	}
	
```

## requestDto
- TokenDto
```java
@Getter
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class TokenDto {
	//권한 부여
	private String grantType; 
	// 보호된 리소스에 액세스하는 데 사용되는 자격 증명
	private String accessToken; 
	//access Token의 유효기간이 지났을 때 새롭게 발급해주는 역할
	private String refreshToken; 
	//access token 만료 시간(유효기간)
	private Long accessTokenExpiresIn; 
}
```

- RoleEnum
```java
public enum Role_Enum {
	POST, COMMENT
}
```

# shared
- Authority
```java
public enum Authority {

	ROLE_USER, //회원
	
	ROLE_GUEST //비회원

}
```

## jwt
- AccessDeniedHandlerException
```java

import io.jsonwebtoken.io.IOException;
import org.springframework.security.access.AccessDeniedException;
import org.springframework.security.web.access.AccessDeniedHandler;
import org.springframework.stereotype.Component;
import sparta.drawmydaily_backend.controller.response.ResponseDto;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@Component
public class AccessDeniedHandlerException implements AccessDeniedHandler {

//권한이 필요한 행동을 로그인 하지 않은 사용자가 사용시 오류 출력
	@Override
	public void handle(HttpServletRequest request, HttpServletResponse response, AccessDeniedException accessDeniedException) throws IOException, java.io.IOException {
	
	response.setContentType("application/json;charset=UTF-8");
	response.getWriter().println(
			new ObjectMapper().writeValueAsString(
			ResponseDto.fail("BAD_REQUEST", "로그인이 필요합니다.")
	
			)
	);
	response.setStatus(HttpServletResponse.SC_FORBIDDEN);
	
	}

}
```

- AuthenticationEntryPointException
```java

//인증 예외 처리
@Component
public class AuthenticationEntryPointException implements AuthenticationEntryPoint {

	@Override
	public void commence(HttpServletRequest request, HttpServletResponse response,
	AuthenticationException authException) throws IOException {
	
	response.setContentType("application/json;charset=UTF-8");
	response.getWriter().println(
			new ObjectMapper().writeValueAsString(
	ResponseDto.fail("BAD_REQUEST", "로그인이 필요합니다.")
	
			)
	
	);
	
	response.setStatus(HttpServletResponse.SC_UNAUTHORIZED);
	
	}

} 

```

- JwtFilter
```java
@RequiredArgsConstructor
public class JwtFilter extends OncePerRequestFilter {
public static String AUTHORIZATION_HEADER = "Authorization";
public static String BEARER_PREFIX = "Bearer ";
public static String AUTHORITIES_KEY = "auth";
private final String SECRET_KEY;
private final TokenProvider tokenProvider;
private final UserDetailsServiceImpl userDetailsService;

	protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws IOException, ServletException{
	
	byte[] keyBytes = Decoders.BASE64URL.decode(SECRET_KEY);
	Key key = Keys.hmacShaKeyFor(keyBytes);
	String jwt = resolveToken(request); //bearer 제외 된 토큰
	
	if (StringUtils.hasText(jwt)&&tokenProvider.validateToken(jwt)){
		Claims claims;
		// getBody()를 이용해 앞서 토큰에 저장했던 data들이 담긴 claims 를 얻어옴
		try {
			claims = Jwts.parserBuilder().setSigningKey(key).build().parseClaimsJws(jwt).getBody(); 
			
		//claims에 key 받아옴(Key키로 서명되었으면, 같은 Key가 JwtParserBuilder에 지정되어야 함)
		}catch (ExpiredJwtException e){
			claims = e.getClaims();
		}
		
		if(claims.getExpiration().toInstant().toEpochMilli() < Instant.now().toEpochMilli()){
		
		response.setContentType("application/json;charset=UTF-8");
		response.getWriter().println(
			new ObjectMapper().writeValueAsString(
				ResponseDto.fail("BAD_REQUEST", "Token이 유효햐지 않습니다.")
		
			)
		
		);
		//받아온 claims의 만료가 현재 저장된 만료보다 짧으면 즉 만료된 token이니 유효하지 않다는 에러
		response.setStatus(HttpServletResponse.SC_BAD_REQUEST);
		
		} 

		//인증 키를 가져와 이를 새 권한을 지정해 map 해줌
		String subject = claims.getSubject();
		Collection<? extends GrantedAuthority> authorities =
		
Arrays.stream(claims.get(AUTHORITIES_KEY).toString().split(","))
		
			.map(SimpleGrantedAuthority::new)
			.collect(Collectors.toList());
		
		
		//principal은 인증된 사용자 정보로 / 즉 위의 과정들이 다 진행된다면 인증(로그인)된 사용자니 이 사용자의 이름에 권한을 지정해 저장해둠
		UserDetails principal = userDetailsService.loadUserByUsername(subject);
		
		//인증된 사용자 정보인 principal은 authenticaion에서 관리하니 이를 사용하기 위해 / 로그인 요청에서 username과 password를 가져와 여기에 인증된 사용자, jwt 토큰 키, 유효기간을 저장해줌)
		Authentication authentication = new UsernamePasswordAuthenticationToken(principal, jwt, authorities);
		
		
		//authentication을 securitycontextholder에서 관리
		//Spring으로 요청이 들어왔을때 헤더의 인증정보를 스레드 내 저장소에 담아놓고 해당 스레드에서 필요 시 꺼내서 사용
	SecurityContextHolder.getContext().setAuthentication(authentication);
		
		}
	
		filterChain.doFilter(request,response);
	
	}


//bearer 로 시작하면 그 뒤에 token 정보가 들어있으니 잘라서 token 정보만 넣어줌
	private String resolveToken(HttpServletRequest request) {
	
		String bearerToken = request.getHeader(AUTHORIZATION_HEADER); //헤더에 넣어줌
		
		if (StringUtils.hasText(bearerToken)
			&& bearerToken.startsWith(BEARER_PREFIX)){
		
		return bearerToken.substring(7);
		
		}
		
		return null;
		
	} 
}
```

- TokenProvider
```java

@Component
@Slf4j
public class TokenProvider {
	//권한을 주기 위한 key 부여
	private static final String AUTHORITIES_KEY = "auth";
	//JWT 혹은 OAuth에 대한 토큰을 사용, 토큰 앞에 Bearer 문자열 필요해서 고정
	private static final String BEARER_PREFIX = "Bearer ";
	
	//엑세스 토큰 만료 = 30분(현 1일)
	private static final long ACCESS_TOKEN_EXPIREE_TIME = 1000 * 60 * 60 * 24; 
	
	//리프레시 토큰 만료 = 7일
	private static final long REFRESH_TOKEN_EXPIRE_TIME = 1000 * 60 * 60 * 24 * 7; 
	
	private final Key key;
	private final RefreshTokenRepository refreshTokenRepository;
	// private final UserDetailsServiceImpl userDetailsService;

	//secret(비밀키)을 decode해서 반환(암호화 해제 해서 확인
	public TokenProvider(@Value("${jwt.secret}") String secretKey, RefreshTokenRepository refreshTokenRepository){
	
		this.refreshTokenRepository = refreshTokenRepository;
		byte[] keyBytes = Decoders.BASE64URL.decode(secretKey);
		this.key = Keys.hmacShaKeyFor(keyBytes);
		
	} )
	
	public TokenDto generateTokenDto(Users users){
		//만료시간 설정을 위해 현재 시간 가져오기
		long now = (new Date().getTime());
	
	
	
	Date accessTokenExpiresIn = new Date(now + ACCESS_TOKEN_EXPIREE_TIME); //access 토큰 만료시간 설정 완료

	//accessToken 생성
	String accessToken = Jwts.builder()
		.setSubject(users.getName())
		.claim(AUTHORITIES_KEY, Authority.ROLE_USER.toString())
		.setExpiration(accessTokenExpiresIn)
		.signWith(key, SignatureAlgorithm.HS256)
		.compact();
	
	
	//refreshToken 생성
	String refreshToken = Jwts.builder()
		.setExpiration(new Date(now + REFRESH_TOKEN_EXPIRE_TIME))
		.signWith(key, SignatureAlgorithm.HS256)
		.compact();

	//refreshToken 생성 후 이를 user에 매핑
	RefreshToken refreshTokenObject = RefreshToken.builder()
		.id(users.getId())
		.users(users)
		.keyValue(refreshToken)
		.build();
	
	refreshTokenRepository.save(refreshTokenObject);
	
	//토큰 생성
	return TokenDto.builder()
		.grantType(BEARER_PREFIX)
		.accessToken(accessToken)
		.accessTokenExpiresIn(accessTokenExpiresIn.getTime())
		.refreshToken(refreshToken)
		.build();
	
	} 
	
	//사용자(유저, 회원가입한 사용자)에게 인증 부여
	public Users getUserFromAuthentication(){
	
		Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
		
		if (authentication == null || AnonymousAuthenticationToken.class.
		
		isAssignableFrom(authentication.getClass())) {
		
		return null;
		
		}
		
		return ((UserDetailsImpl) authentication.getPrincipal()).getUsers();
		
		} 

	//토큰유효성 검사(유효, 만료, 지원, 잘못된 토큰인지 예외처리)
	public boolean validateToken(String token){
	
		try{
				Jwts.parserBuilder().setSigningKey(key).build().parseClaimsJws(token);
		
		return true;
		
		} catch (SecurityException | MalformedJwtException e){
		
		log.info("Invalid JWT signature, 유효하지 않는 JWT 서명 입니다.");
		
		} catch (ExpiredJwtException e) {
		
		log.info("Expired JWT token, 만료된 JWT token 입니다.");
		
		} catch (UnsupportedJwtException e) {
		
		log.info("Unsupported JWT token, 지원되지 않는 JWT 토큰 입니다.");
		
		} catch (IllegalArgumentException e) {
		
		log.info("JWT claims is empty, 잘못된 JWT 토큰 입니다.");
		
		}
		
		return false;
		
		} 

	//현재 있는 refreshtoken인지 확인
	@Transactional(readOnly = true)
	public RefreshToken isPresentRefreshToken(Users users){
	
		Optional<RefreshToken> optionalRefreshToken = refreshTokenRepository.findByUsers(users);
		
		return optionalRefreshToken.orElse(null);
		
	}

	//refreshtoken 삭제
	@Transactional
	public ResponseDto<?> deleteRefreshToken(Users users) {
	
		RefreshToken refreshToken = isPresentRefreshToken(users);
		if (null == refreshToken) {
		
		return ResponseDto.fail("TOKEN_NOT_FOUND", "존재하지 않는 Token 입니다.");
		
		}
		
		refreshTokenRepository.delete(refreshToken);
		
		return ResponseDto.success("success");
		
		} 

}
```

## controller
- CustomExceptionHandler
```java
@RestControllerAdvice
public class CustomExceptionHandler {

	//예외처리, request가 잘못들어오면 출력
	@ExceptionHandler(MethodArgumentNotValidException.class)
	public ResponseDto<?> handleValidationExceptions(MethodArgumentNotValidException exception){
	
	String errorMessage = exception.getBindingResult()
		.getAllErrors()
		.get(0)
		.getDefaultMessage();
	
	return ResponseDto.fail("BAD_REQUEST", errorMessage);
	
	} 

}
```

- ResponseDto
```java
@Getter
@AllArgsConstructor
public class ResponseDto<T> {

	private boolean success;
	private T data;
	private Error error;
	//응답이 성공적으로 되면 success true 메세지와, 해당 데이터, 에러 없음 표현
	public static <T> ResponseDto<T> success(T data){
	
	return new ResponseDto<>(true,data, null );
	
	
	}

	//응답 실패시 false메세지와 데이터 null 해당 에러 출력
	public static <T> ResponseDto<T> fail(String code, String message){
	
	return new ResponseDto<>(false, null, new Error(code, message));
	
	
	}

	//에러를 출력하기 위해 class 설정
	@Getter
	@AllArgsConstructor
	static class Error{
	private String code;
	private String message;
	
	}

}
```

## repository
- RefreshTokenRepository
```java
public interface RefreshTokenRepository extends JpaRepository<RefreshToken, Long> {

	Optional<RefreshToken> findByUsers(Users users);

}
```

## Service
- UserDetailsServiceImpl
```java
@Service
@RequiredArgsConstructor

//사용자 이름(회원가입시 가입한 id) 넘겨주기, 해당 이름 없으면 가입된 유저가 없으니 사용자 찾을 수 없다 에러
public class UserDetailsServiceImpl implements UserDetailsService {

	private final UsersRepository usersRepository;
	
	@Override
	public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
	
		Optional<Users> user = usersRepository.findByName(username);
	
		return user
		.map(UserDetailsImpl::new)
		.orElseThrow(()->new UsernameNotFoundException("사용자를 찾을 수 없습니다."));
	
	}

}
```