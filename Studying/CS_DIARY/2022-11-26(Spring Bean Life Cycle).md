# Spring Bean Life Cycle

- 스프링 빈이란, 개발자가 new 연산자를 통해 직접 생성하고 관리하는 객체가 아니라 Spring IoC Container가 관리하는 객체를 뜻한다.
- @Component 어노테이션이나 설정 클래스를 만들어서 @Configuration 어노테이션을 클래스 선언부에 추가한 후 메서드에 @Bean 어노테이션을 붙여줌으로써 등록할 수 있고 필요한 곳에서 @Autowired 등을 통해 의존성을 주입받아 사용할 수 있다.
- 순서는 스프링 컨테이너 생성 > 스프링 빈 생성 > 의존관계 주입 > 초기화 콜백(EVENT) > 역할 수행 > 소멸 전 콜백(EVENT) > 스프링 종료이다.
- 스프링 컨테이너가 가동되고 역할을 수행하기 전에 한 번, 종료하기 전에 한 번 특정한 동작을 수행할 수 있는 이벤트가 존재하는데, 이러한 이벤트에서 초기화 콜백(@PostConstruct)을 이용해서 테스트로 사용할 데이터를 미리 저장한다던가 소멸전 콜백(@PreDestroy)을 이용해 스프링이 종료되기 전 데이터를 백업하는 등의 동작을 수행할 수 있다.

---
### 연관자료
- [[IoC(Inversion_of_Control)와_DI(Dependency_Injection)_&_Bean]]
- [DI-의존성주입]