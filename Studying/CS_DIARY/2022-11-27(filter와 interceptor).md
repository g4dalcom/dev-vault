- `filter` 와 `interceptor` 는 `AOP` 와 함께 공통 기능을 모아서 처리할 수 있는 방법을 제공한다.
- `filter` 는 `Dispatcher Servlet에 요청이 전달되기 전/후` 로 스프링 컨테이너가 아닌 톰캣과 같은 `웹 컨테이너` 에 의해 관리된다. Fileter 인터페이스에는 필터 객체를 초기화하고 서비스에 추가하기 위한 init() 메서드, 전/후 처리를 위한 doFilter() 메서드, 종료를 위한 destory() 메서드가 있다.
- `interceptor` 는 `Controller` 를 호출하기 전/후에 `스프링 컨텍스트` 에서 동작한다. `HandlerInterceptor` 의 메서드로는 Controller가 호출되기 전에 하는 전처리 작업을 위한 preHandle(), 후처리 작업을 위한 postHandle(), View 렌더링 후 리소스를 반환하기 위한 afterCompletion() 메서드가 있다.
- 필터는 스프링과 무관하게 전역적으로 처리해야 하는 보안 검사, 이미지나 데이터의 압축, 문자열 인코딩과 같은 웹 어플리케이션 전반에 사용되는 기능을 구현할 때 사용하고
- 인터셉터는 로그인 체크와 같은 세부적인 보안 작업이나 JWT 토큰 정보를 파싱해서 컨트롤러로 넘겨주는 등의 작업에 사용된다.

---
### 연관자료
- [필터와 인터셉터의 개념과 차이](https://dev-coco.tistory.com/173)
