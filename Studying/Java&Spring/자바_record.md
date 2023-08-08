# record 란 무엇일까요?

- **불변(immutable) 데이터 객체**를 쉽게 생성할 수 있도록 하는 새로운 유형의 클래스로 일반 class와 다르게 **생성자, getter, hashCode(), equals(), toString()** 메소드를 제공해줍니다.
- **DTO**처럼 데이터를 운반하는 특성의 클래스를 구현할 때 반복적으로 작성했던 부분을 없애줍니다.
- JDK14 버전에서 preview로 등장하여 JDK16 버전에서 정식 스펙으로 포함되었습니다.

# 어떻게 사용하는 걸까요?

### 선언 방식

```java
record 레코드명(컴포넌트1, 컴포넌트2) {}
```

### class를 사용한 DTO 객체

```java
public class Person {
	private final String name;
	private final int age;

	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}

	public String getName() {
		return name;
	}

	public int getAge() {
		return age;
	}

	// ... //
	
	@Override
	public int hashCode() {
		return Objects.hash(name, age);
	}
}
```

### record 를 이용한 DTO 객체

```java
public record Person(String name, int age) {};
```

- record가 대신해주는 부분은 다음과 같습니다.
	- 모든 컴포넌트를 **private final**로 필드 선언
	- 모든 파라미터를 포함하는 생성자 선언
	- 모든 필드의 get 메소드 생성
	- 로깅 출력을 위한 toString, 인스턴스 비교를 위한 hashCode, equals 메소드 정의


## 적용해보기

```java
public class LoginDto {

    @Getter
    @NoArgsConstructor(access = AccessLevel.PROTECTED)
    public static class Request {
        private String username;
        private String password;

        @Builder
        public Request(final String username, final String password) {
            this.username = username;
            this.password = password;
        }
    }

    @Getter
    @NoArgsConstructor(access = AccessLevel.PROTECTED)
    public static class Response {
        private Long member_id;
        private String username;
        private String nickname;
        private MemberRoleEnum role;
        private PlatformEnum platform;

        @Builder
        public Response(final Long member_id, final String username, final String nickname, final MemberRoleEnum role, final PlatformEnum platform) {
            this.member_id = member_id;
            this.username = username;
            this.nickname = nickname;
            this.role = role;
            this.platform = platform;
        }

        public static Response of(Member member) {
            return Response.builder()
                    .member_id(member.getId())
                    .username(member.getUsername())
                    .nickname(member.getNickname())
                    .role(member.getRole())
                    .platform(member.getPlatform())
                    .build();
        }
    }
}
```

- 개인 프로젝트에서 사용했던 LoginDto 부분입니다. Request와 Response를 innerClass로 구현하였습니다.
- 만약 또 다른 DTO를 만든다면 **필드 선언, @Getter, @NoArgsConstructor 어노테이션 선언, 빌더 패턴 생성자 선언** 의 작업을 똑같이 해주어야 합니다.
- 그외 toString과 같은 메소드도 사용한다면 반복적으로 작성을 해주어야겠죠.
- 앞서 보았던 예시처럼 보일러 플레이트를 작성하지 않는 record 형식으로 리팩토링해보겠습니다.


## DTO 리팩토링

```java
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public static class Request {
	private String username;
	private String password;

	@Builder
	public Request(final String username, final String password) {
		this.username = username;
		this.password = password;
	}
}
```

- class에서 필드 선언과 생성자를 만들었던 부분을 record에서는 단 한 줄로 간단하게 표현할 수 있습니다.

```java
@Builder
public record Request(String username, String password) {}
```

- class에서 파라미터라고 부르는 부분을 컴포넌트라고 하는데 컴포넌트를 record 괄호 안에 정의해주면 **private final**로 필드가 선언된 것처럼 만들어주고 **AllArgsConstructor** 어노테이션을 사용한 것과 같은 작업을 해줍니다.
- 저같은 경우는 빌더 패턴을 애용하기 때문에 @Builder 어노테이션을 붙여서 커스텀하였습니다.

```java
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public record LoginDto() {
    @Builder
    public record Request(String username, String password) {}

    @Builder
    public record Response(Long member_id, String username, String nickname, MemberRoleEnum role, PlatformEnum platform) {
        public static Response of(Member member) {
            return Response.builder()
                    .member_id(member.getId())
                    .username(member.getUsername())
                    .nickname(member.getNickname())
                    .role(member.getRole())
                    .platform(member.getPlatform())
                    .build();
        }
    }
}
```

- 메소드를 정의하는 것도 동일하게 record 안에서 할 수 있습니다.

### MemberService

```java
public LoginDto.Response login(LoginDto.Request request) {  
    Member member = memberRepository.findByUsername(request.username())  
            .orElseThrow(() -> new CustomException(ErrorCode.NOT_FOUND_USER));
```

- class와 또 다른 차이는 get메소드를 사용할 때입니다.
- class는 `request.getUsername()` 과 같이 get + 필드명을 사용하지만 record는 `request.username()` 처럼 **필드명을 그대로** 사용하게 됩니다.

# 그 외 record 제약 사항

- 레코드는 기본적으로 java.lang.Record 클래스를 상속받기 때문에 다른 클래스를 상속받을 수 없습니다.
- private final 이외의 인스턴스 필드를 선언할 수 없습니다. 다시 말하면 모든 필드는 static이어야 한다는 것입니다.
- 또한 암시적으로 final 이며 추상적(abstract)일 수 없습니다.
- 이러한 레코드의 불변한 상태 조건은 DTO와 같은 불변 데이터 객체를 다루기 적합하다는 것을 의미하는 것 같습니다.

- 위 제약사항 이외에 inner class, 제네릭, 인터페이스 구현 여부, new 키워드를 통한 인스턴스화와 같은 특징들은 모두 일반 class와 동일합니다.

---

### 관련 자료

- [DTO를 record type으로 설정하는 이유](https://velog.io/@oyoungsun/Java-RECORD-DTO%EB%A5%BC-record-type%EC%9C%BC%EB%A1%9C-%EC%84%A4%EC%A0%95%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0)
- [자바의 레코드(Record)](https://scshim.tistory.com/372)
- [Java16 - record](https://cyk0825.tistory.com/75)
- [Java 레코드용 빌더 패턴](https://howtodoinjava.com/java/basics/builder-pattern-for-java-records/)
- [레코드(Record)](https://blog.hexabrain.net/399)
- [Record Class](https://velog.io/@rmswjdtn/JAVA-%EB%AC%B8%EB%B2%95-Record-Class)