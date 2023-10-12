# JUnit이란?

- JUnit 은 Java 진영의 대표적인 **단위 테스트 프레임워크**입니다.
- 어노테이션을 기반으로 테스트를 지원하며 스프링부트 프로젝트의 경우 기본적으로 의존성이 추가되어 있습니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbbdmao%2FbtsydWkglUp%2FKJvO4h8OySfYZ0QnOpWVXk%2Fimg.png)

- JUnit 5는 크게 세 가지 모듈로 이루어져 있습니다.
- **JUnit Platform**은 테스트를 발견하고 테스트 계획을 생성하는 TestEngine API를 지원해줍니다. 테스트를 실행하기 위한 뼈대라고 볼 수 있습니다.
- **JUnit Jupiter**는 TestEngine API 구현체로 개발자가 테스트 코드를 작성할 때 사용되며 테스트 코드를 발견하고 실행하는 역할을 수행합니다.
- **JUnit Vintage**는 JUnit 이전 버전은 3, 4와의 호환성을 위한 모듈입니다.

# 주요 어노테이션

## LifeCycle Annotation

### @BeforeAll

- 해당 클래스에 위치한 모든 테스트 메소드 **실행 전**에 **딱 한 번** 실행되는 메소드

### @AfterAll

- 해당 클래스에 위치한 모든 테스트 메소드 **실행 후**에 **딱 한 번** 실행되는 메소드

### @BeforeEach

- 각 테스트 메소드들이 시작되기 전에 실행되는 메소드

### @AfterEach

- 각 테스트 메소드들이 시작된 후 실행되는 메소드

> --Each 메소드의 경우 매 테스트 메소드마다 새로운 클래스를 생성(new) 합니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbHZ4T1%2FbtsymO6u6vf%2FowMOBNZUDgJPWSRVq81s00%2Fimg.png)

- 위와 같이 라이프사이클 어노테이션을 테스트 메소드와 함께 실행해보았습니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FvritT%2FbtsyeoAI03L%2F8zGrdJKW9L74Oee5Mzs4tk%2Fimg.png)

- 가장 먼저 beforeAll이 한 번 실행되고 각 테스트 실행 전에 beforeEach, 실행 후에 afterEach가 실행되고 마지막에 afterAll이 한 번 실행되는 것을 볼 수 있습니다.
- 더불어 @Disabled 어노테이션이 붙은 테스트 케이스는 라이프사이클 어노테이션도, 테스트 메소드도 실행되지 않고 무시되는 것을 확인할 수 있습니다.


## Annotation

### @Test

-  테스트 메소드라는 것을 명시해주는 어노테이션

### @Disabled

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb6f1Gf%2FbtsycXXZTwV%2FTtdvkkwRJ1KjYQVpMo4hik%2Fimg.png)

- 테스트를 하지 않을 메소드에 붙여주는 어노테이션입니다.
- 테스트 결과에서 테스트를 진행하지 않았다고 표시가 됩니다.

### @DisplayName

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdAm7cn%2Fbtsyl0lGDWA%2FauACSKP2Rpjq7scALA5MZ1%2Fimg.png)

- IDE나 빌드툴에서 결과값으로 기본적으로 테스트 메소드명이 노출되는데 이러한 테스트명을 표현할 수 있게 해주는 어노테이션입니다.
- 공백, Emoji, 특수문자 등을 모두 지원해주기 때문에 보다 직관적으로 테스트명을 지정해줄 수 있습니다.

### @RepeatedTest

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FwKEsp%2Fbtsylz23wDq%2Fdw8Hjf7nrL9ht4ic1K5AY0%2Fimg.png)

- 특정 테스트를 반복하여 성능적인 이슈를 확인할 때 사용하는 어노테이션입니다.
- **displayName**, **currentRepetition**, **totalRepetitions**를 이용하면 각 반복마다 테스트명을 커스텀하게 출력할 수도 있습니다.

### @ParameterizedTest

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbN1Kau%2FbtsydTnxjey%2F2KTRrIB0Ap2WJszJcSzOK0%2Fimg.png)

- 테스트에 여러 다른 매개변수를 대입해가며 반복 실행할 때 사용하는 어노테이션입니다.
- 여러 개의 매개변수는 @CsvSource 어노테이션을 이용해 value값으로 넣을 수 있습니다. **{"input", "expected"}** 의 형태로 기본 구분자는 **","** 입니다.
- delimiter 값으로 구분자를 커스텀할 수도 있습니다.
- 이 외에도 매개변수로 null값을 주입해주는 @NullSource, 빈 값을 주입해주는 @EmptySource와 Enum 값에 대한 테스트용인 @EnumSource 등 여러 어노테이션이 지원됩니다.


## Main Annotation

### @SpringBootTest

- 통합 테스트 용도로 사용되며 @SpringBootApplication의 하위 Bean들을 스캔하여 로드합니다.
- Test용 Application Context를 만들어서 Bean에 추가하고 MockBean을 찾아서 교체하는 작업을 수행합니다.

### @ExtendWith

- 메인으로 실행될 Class를 지정할 수 있습니다.

### @WebMvcTest(Class명.class)

- 명시된 클래스만 로드하여 테스트를 진행합니다.
- 매개변수를 지정해주지 않으면 @Controller, @RestController, @RestControllerAdvice 등 컨트롤러와 연관된 Bean이 모두 로드됩니다.
- 스프링의 모든 Bean을 로드하는 @SpringBootTest 대신 컨트롤러 관련 코드만 테스트할 경우에 사용합니다.

### @Mockbean

- 테스트할 클래스에서 주입받고 있는 객체에 대해 가짜 객체를 생성해주는 어노테이션입니다.
- 해당 객체는 실제 행위를 하지 않고 가짜 객체의 동작에 대해 정의하여 제어할 수 있습니다.

# Assertions(단정문)

- 테스트 케이스의 수행 결과를 판별하는 메소드로써 모든 JUnit Jupiter Assertions는 static 메소드입니다.
- 수행된 결과가 **isEqualTo()** 의 결과와 같은지를 판별하여 테스트 통과 여부가 결정됩니다.

### assertThat, assertAll, assertSoftly

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fw3kAU%2FbtsybUAoxjI%2FiYZlgMAVJZeviphe4yk7ok%2Fimg.png)

- assertThat은 중간에 다른 결과값이 나오면 이후 테스트는 진행하지 않고 테스트를 종료합니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FR839m%2FbtsynCELgV8%2F4UuUFtWAMD0eUJh1J0vAt1%2Fimg.png)

- assertAll은 모든 테스트 케이스를 진행한 후 실패한 테스트에 대한 예상값과 실제값을 출력해줍니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FczUOHr%2FbtsycUNLUvX%2FoyLbcKkaVuGTTY4Q04PNw0%2Fimg.png)

- assertSoftly도 assertAll과 마찬가지로 모든 테스트 케이스를 진행한 후 실패한 테스트에 대한 메시지를 출력해주지만 어디서 실패했는지 코드 라인의 위치까지 알려준다는 점에 차이가 있습니다.


---

### 참고 자료

- [@ParameterizedTest로 한 번에 테스트하자](https://velog.io/@ohzzi/junit5-parameterizedtest)
- [어라운드 허브 스튜디오 - 테스트 코드 적용하기 (JUnit, TDD) 스프링 부트 (Spring Boot)](https://youtu.be/SFVWo0Z5Ppo?si=ky9CQGyJVLFBgUyP)
- [10분 테코톡 - JUnit5 사용법](https://youtu.be/EwI3E9Natcw?si=_XWsSA8JByjCwtIY)
- [JUnit5 assertThat vs assertAll vs assertSoftly](https://zzang9ha.tistory.com/418)