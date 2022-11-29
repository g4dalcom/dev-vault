# AOP

- 로그를 남긴다거나 권한을 체크하는 등 일종의 부가기능들은 애플리케이션 전역에서 나타날 수 있는데, 이것을 핵심 기능인 비즈니스 로직에 넣게 되면 중복코드가 많아지고 유지보수가 어려워진다.
- 로깅이나 권한 검사와 같은 부가기능들은 하나의 관심사를 갖게 되는데 비즈니스 로직을 수행할 때 이러한 관심사들이 비즈니스 로직마다 중복이 되어 횡단으로 나타나기 때문에 이것을 횡단 관심사라고 한다.
- AOP는 관점지향프로그래밍으로, 핵심 기능인 비즈니스 로직과 부가기능을 분리해서 부가기능인 횡단관심사들을 모듈화해서 객체지향을 보완할 수 있도록 하는 것이 목적이다.
- 서비스 로직을 하나의 트랜잭션으로 만들 때 원래는 로직의 시작점에 트랜잭션을 열어주고 끝날 때 트랜잭션을 커밋하는 코드가 들어가야 하는데 @Transactional 어노테이션만으로 비즈니스 로직에 집중할 수 있고 예외처리 같은 경우 @RestControllerAdvise 어노테이션을 이용해 컨트롤러에서 발생하는 모든 예외를 잡고 ExceptionHandler가 개별적으로 예외를 잡아서 처리할 수 있다.

# Spring IoC Container

- 객체의 생명주기를 개발자가 아니라 스프링 컨테이너가 제어를 해준다고 해서 `개발자에서 스프링으로 제어가 역전`되었다는 의미가 IoC, 제어의 역전이다.
- 그리고 이러한 객체, 빈을 관리하는 주체가 스프링 IoC Container 이다.
- IoC Container는 객체 간 결합도를 낮추고 유연한 코드를 작성할 수 있도록 해주고 덕분에 외부에서 의존성을 주입하는 DI와 관점지향 프로그래밍을 가능하게 한다. 
- 스프링 컨테이너의 최상위 인터페이스로는 `beanFactory` 가 있고 빈팩토리를 모두 상속받아서 여러 부가 기능을 제공하는 `ApplicationContext` 가 있다.

---
### 연관자료
- [AOP의 개념과 적용방법](https://creamilk88.tistory.com/148)
- [스프링AOP, Aspect 개념 특징](https://shlee0882.tistory.com/206)
- [우테코 Spring AOP](https://www.youtube.com/watch?v=Hm0w_9ngDpM&t=222s)
- [SpringBean, IoC Container, DI가 뭔데?!](https://velog.io/@jkijki12/Spring-%EC%8A%A4%ED%94%84%EB%A7%81-Bean-IoC-Container-DI%EA%B0%80-%EB%AD%94%EB%8D%B0)
