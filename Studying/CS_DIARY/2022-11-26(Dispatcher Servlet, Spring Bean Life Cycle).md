# Spring WEb MVC의 Dispatcher Servlet 동작원리

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FHgZeJ%2FbtrfoTlO2uZ%2F14G2NA51AuEf8sSpemccv0%2Fimg.jpg)

- 우선 디스패처 서블릿은 서블릿 컨테이너(톰캣 등)을 통해 생명주기가 관리됩니다. 이러한, 싱글톤 빈인 디스패처 서블릿은 프론트 컨트롤러로, 클라이언트 요청들을 받습니다.
- 이후 핸들러 매핑을 통해 요청을 처리할 컨트롤러(핸들러)를 찾고, 핸들러 어댑터를 통해 웹어플리케이션컨텍스트 속에서 검색된 컨트롤러로 요청을 전달합니다. 
- 이후 웹어플리케이션컨텍스트를 통해 서비스, DB 빈들이 호출되며 비즈니스 로직이 수행됩니다. 컨트롤러의 리턴값 역시 핸들러 어댑터를 통해 뷰리졸버에 전달되고, 안에서 파라미터 값에 대한 ModelAndView 등이 반환됩니다. 해당 객체는 뷰로 전달되고요.
- 디스패처 서블릿은 최종적으로 뷰에서 전달받은 리턴값을 클라이언트에게 응답해줍니다.  
- 이렇듯 디스패처 서블릿은 요청/응답에 대한 전처리 및, 컨트롤러 매핑 등 공통 로직을 수행합니다. 이를 통해 개발자는 비즈니스 로직에 집중한 구현이 가능합니다.

---

- Dispatcher Servlet이 호출되면 가장 먼저 `doService()` 메서드가 실행되고 이 메서드는 `doDispatch()` 메서드를 호출한다.
- `doDispatch()` 는 `getHandler()` 를 통해 클라이언트 요청에 해당하는 핸들러를 조회하고, `getHandlerAdapter()` 를 통해 해당 핸들러를 받는다. 
- `getHandlerAdapter()` 를 통해 받은 어댑터의 `handle()` 메서드를 실행하고 `handle()` 메서드는 `processDispatchResult()` 메서드를 실행한다.
- `processDispatchResult()` 는 `render()` 메서드를 호출하고 `render()` 메서드는 `viewResolver` 를 통해 `view` 를 찾아서 전달한다.


# Spring Bean Life Cycle

- 스프링의 IoC 컨테이너는 Bean 객체들을 책임지고 의존성을 관리한다.  
- 객체들을 관리한다는 것은 객체의 생성부터 소멸까지의 생명주기(LifeCycle) 관리를 개발자가 아닌 컨테이너가 대신 해준다는 말이다.  
- 객체 관리의 주체가 프레임워크(Container)가 되기 때문에 개발자는 로직에 집중할 수 있는 장점이 있다.
- @Component 어노테이션이나 설정 클래스를 만들어서 @Configuration 어노테이션을 클래스 선언부에 추가한 후 메서드에 @Bean 어노테이션을 붙여줌으로써 등록할 수 있고 필요한 곳에서 @Autowired 등을 통해 의존성을 주입받아 사용할 수 있다.
- 순서는 스프링 컨테이너 생성 > 스프링 빈 생성 > 의존관계 주입 > 초기화 콜백(EVENT) > 역할 수행 > 소멸 전 콜백(EVENT) > 스프링 종료이다.
- 스프링 컨테이너가 가동되고 역할을 수행하기 전에 한 번, 종료하기 전에 한 번 특정한 동작을 수행할 수 있는 이벤트가 존재하는데, 이러한 이벤트에서 초기화 콜백(@PostConstruct)을 이용해서 테스트로 사용할 데이터를 미리 저장한다던가 소멸전 콜백(@PreDestroy)을 이용해 스프링이 종료되기 전 데이터를 백업하는 등의 동작을 수행할 수 있다.


---
### 연관자료
- [[Dispatcher-Servlet]]
- [Dispatcher Servlet 동작원리](https://velog.io/@ejung803/Spring-Web-MVC%EC%9D%98-Dispatcher-Servlet%EC%9D%98-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC)
- [Dispatcher Servelt 코드로 보는 동작원리](https://yejun-the-developer.tistory.com/m/4)
- [[IoC(Inversion_of_Control)와_DI(Dependency_Injection)_&_Bean]]
- [DI-의존성주입]