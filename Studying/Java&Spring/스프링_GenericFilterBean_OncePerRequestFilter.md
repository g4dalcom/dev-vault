- Spring Security와 JWT를 활용해서 회원 인증 기능을 구현하기 위해 여러 코드들을 보고 공부하던 중에 
- `GenericFilter`를 이용해 필터를 구현한 코드와 `OncePerRequestFilter`를 이용한 코드가 있어서 서로 어떤 차이가 있는지 궁금해서 찾아보았다.

# 개요

- GenericFilter와 OncePerRequestFilter는 둘 다 대상을 필터로 등록해주는 인터페이스이다.

```java
public class JwtFilter extends GenericFilterBean {

    @Override
    public void doFilter(final ServletRequest request, final ServletResponse response, final FilterChain chain) throws IOException, ServletException {
        
    }
}
```

```java
public class JwtFilter extends OncePerRequestFilter {

    @Override
    protected void doFilterInternal(final HttpServletRequest request, final HttpServletResponse response, final FilterChain filterChain) throws ServletException, IOException {
        
    }
}
```

### Filter 동작 흐름
- **Filter**는 `javax.servlet-api`나 `tomcat-embed-core`를 사용하면 제공되는 Servlet Filter Interface로써 클라이언트의 서블릿 요청을 가장 먼저 받는다.

```java
    @Override
    public void doFilter(final ServletRequest request, final ServletResponse response, final FilterChain chain) throws IOException, ServletException {

		// do Filter 실행 전 로직
        println("Before")
        chain?.doFilter(request, response) ?: throw Exception()
        // do Filter 실행 후 로직
        println("After")

    }
```
- 서블릿이 호출되기 전에 Before를 출력하고 필터를 거쳐서 서블릿이 호출되면 After가 출력된다.
- 이러한 Filter를 확장하여 Spring의 설정정보를 가져올 수 있게 만들어진 것이 GenericFilterBean이다. 

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FetK4in%2FbtrVLuEhLM5%2F1YGOQfhnOXKQV7mYfyh0N1%2Fimg.png)
- Filter와 GenericFilterBean은 둘 다 매 서블릿마다 호출이 된다.
- 서블릿은 사용자의 요청을 받으면 서블릿을 생성해 메모리에 저장해두고, 같은 클라이언트의 요청을 받으면 생성해둔 서블릿 객체를 재활용하여 요청을 처리한다.
- 문제는 이 서블릿이 다른 서블릿으로 `dispatch`되는 경우이다.
- 가장 대표적으로 `Spring Security`에서 인증과 접근 제어 기능이 `Filter`로 구현되는데 이러한 인증과 접근 제어는 `RequestDispatcher` 클래스에 의해 다른 서블릿으로 `dispatch` 되고, 이 때 이동할 서블릿에 도착하기 전에 다시 한번 `filter chain`을 거치게 된다.(Target API1 -> Target API2)
- 한 번의 요청에 한 번의 인증 처리만 하면 되는데 불필요하게 여러번 중복되어 인증처리를 하게 되는 것이다.
- 이런 문제를 해결하기 위해 등장한 것이 모든 서블릿에 일관된 요청을 처리하기 위해 만들어진 `OncePerRequestFilter`이다.
- 이 추상 클래스를 구현한 필터는 `사용자의 한 번의 요청 당 딱 한 번만` 실행되는 필터를 만들 수 있다.

---
### 관련 자료
- [OncePerRequestFilter와 Filter의 차이](https://minkukjo.github.io/framework/2020/12/18/Spring-142/)
- [# OncePerRequestFilter와 GenericFilterBean](https://velog.io/@chrkb1569/OncePerRequestFilter%EC%99%80-GenericFilterBean)