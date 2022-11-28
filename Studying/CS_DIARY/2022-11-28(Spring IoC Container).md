# Spring IoC Container

- 객체의 생명주기를 개발자가 아니라 스프링 컨테이너가 제어를 해준다고 해서 `개발자에서 스프링으로 제어가 역전`되었다는 의미가 IoC, 제어의 역전이다.
- 그리고 이러한 객체, 빈을 관리하는 주체가 스프링 IoC Container 이다.
- IoC Container는 객체 간 결합도를 낮추고 유연한 코드를 작성할 수 있도록 해주고 덕분에 외부에서 의존성을 주입하는 DI와 관점지향 프로그래밍을 가능하게 한다. 
- 스프링 컨테이너의 최상위 인터페이스로는 `beanFactory` 가 있고 빈팩토리를 모두 상속받아서 여러 부가 기능을 제공하는 `ApplicationContext` 가 있다.

---
### 연관자료
- [SpringBean, IoC Container, DI가 뭔데?!](https://velog.io/@jkijki12/Spring-%EC%8A%A4%ED%94%84%EB%A7%81-Bean-IoC-Container-DI%EA%B0%80-%EB%AD%94%EB%8D%B0)
