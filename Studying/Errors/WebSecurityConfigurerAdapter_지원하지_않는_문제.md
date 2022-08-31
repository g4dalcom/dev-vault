---
keyword : Spring, Security
class : ERROR
---


### WebSecurityConfigurerAdapter 지원하지 않는 문제


#### 에러내용

Spring Security에서 WebSecurityConfigurerAdapter를 extends 하려고 할 때 취소선이 그어지면서 extends 되지 않았다.


---

#### 원인

- Spring Security 5.7 이상에서 더이상 WebSecurityConfiruerAdapter를 지원하지 않음


---

#### 해결

- SecurityFilterChain Bean 등록을 통해 해결

### WebSecurityConfig (기존)
```java
@Configuration
@EnableWebSecurity // 스프링 Security 지원을 가능하게 함
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

		@Override
		public void configure(WebSecurity web) {
				// h2-console 사용에 대한 허용 (CSRF, FrameOptions 무시)
				web
				.ignoring()
				.antMatchers("/h2-console/**");
				}
				
				@Override
				protected void configure(HttpSecurity http) throws Exception {
						// 회원 관리 처리 API (POST /user/**) 에 대해 CSRF 무시
						http.csrf()
						.ignoringAntMatchers("/user/**");
						
						http.authorizeRequests()
						// image 폴더를 login 없이 허용
						.antMatchers("/images/**").permitAll()
						// css 폴더를 login 없이 허용
						.antMatchers("/css/**").permitAll()
						// 회원 관리 처리 API 전부를 login 없이 허용
						.antMatchers("/user/**").permitAll()
						// 그 외 어떤 요청이든 '인증'
						.anyRequest().authenticated()
						.and()
								// 로그인 기능
								.formLogin()
								.loginPage("/user/login")
								.defaultSuccessUrl("/")
								.failureUrl("/user/login?error")
								.permitAll()
						.and()
								// 로그아웃 기능
								.logout()
								.permitAll();
		}
}
```


### WebSecuriryConfig (변경)
```java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig {

    @Bean
    public WebSecurityCustomizer webSecurityCustomizer() {
        return (web) -> web.ignoring().antMatchers("/h2-console/**");
    }

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.csrf()
                .ignoringAntMatchers("/user/**");

        http.authorizeRequests()
                .antMatchers("/images/**").permitAll()      // image 폴더를 login 없이 허용
                .antMatchers("/css/**").permitAll()         // css 폴더를 login 없이 허용
                .antMatchers("/user/**").permitAll()        // 회원 관리 처리 API 전부를 login 없이 허용
                .anyRequest().authenticated()                          // 그 외 어떤 요청이든 '인증'
                .and()
                // 로그인 기능
                    .formLogin()
                    .loginPage("/user/login")
                    .defaultSuccessUrl("/")
                    .failureUrl("/user/login?error")
                    .permitAll()
                .and()
                // 로그아웃 기능
                    .logout()
                    .permitAll();

        return http.build();
    }
}
```
---

#### 연결문서

- [스프링공식문서](https://spring.io/blog/2022/02/21/spring-security-without-the-websecurityconfigureradapter)
