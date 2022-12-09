# Spring WEb MVC의 Dispatcher Servlet 동작원리

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FHgZeJ%2FbtrfoTlO2uZ%2F14G2NA51AuEf8sSpemccv0%2Fimg.jpg)

- 클라이언트의 요청을 Dispatcher Servlet이 최초로 받고 요청 정보를 통해 위임할 컨트롤러를 찾기위해 `HandlerMapping` 에게 물어본다. 
- `HandlerMapping` 에서 해당 컨트롤러를 알려주면 `HandlerAdapter` 에게 위임하고 `HandlerAdapter` 가 `Controller` 에게 처리를 요청한다.
- `Controller` 는 서비스를 호출하여 비즈니스 로직을 처리하고 처리된 요청은 `HandlerAdapter` 가 ModelAndView 객체로 변환해서  `dispatcher servlet` 에게 전달한다.
- dispatcher servlet은  `ViewResolver` 를 통해 필요한 뷰를 전달받아서 내용을 생성하고 클라이언트에게 응답을 한다.

---

- Dispatcher Servlet이 호출되면 가장 먼저 `doService()` 메서드가 실행되고 이 메서드는 `doDispatch()` 메서드를 호출한다.
- `doDispatch()` 는 `getHandler()` 를 통해 클라이언트 요청에 해당하는 핸들러를 조회하고, `getHandlerAdapter()` 를 통해 해당 핸들러를 받는다. 
- `getHandlerAdapter()` 를 통해 받은 어댑터의 `handle()` 메서드를 실행하고 `handle()` 메서드는 `processDispatchResult()` 메서드를 실행한다.
- `processDispatchResult()` 는 `render()` 메서드를 호출하고 `render()` 메서드는 `viewResolver` 를 통해 `view` 를 찾아서 전달한다.


# Spring Bean Life Cycle

- 스프링 빈이란, 개발자가 new 연산자를 통해 직접 생성하고 관리하는 객체가 아니라 Spring IoC Container가 관리하는 객체를 뜻한다.
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