- `Lombok`은 어노테이션을 이용해서 컴파일 시점에 자동으로 코드를 추가할 수 있는 라이브러리이다. 반복해서 작성해야 하는 코드를 알아서 생성해주기 때문에 생산성이 늘어나고 코드 가독성 또한 좋아진다.
- 처음 스프링부트를 접하고부터 롬복을 사용하였는데 당연히 써야되는 것처럼 사용했고 롬복이 제공하는 기능들이 당연한 것처럼 여겨졌다. 
- 그러나 여러 자료들을 접하다 @Data 는 엔티티에서 사용하면 안 된다거나 @Setter 사용을 지양해야 한다거나 하는 얘기들을 들을 수 있었고 우아한테크코스 팀들의 프로젝트 코드를 보면 실제로 롬복을 최소화해서 사용하는 것을 볼 수 있었다.
- 그래서 롬복의 어떤 기능을, 어느정도까지 사용하는 것이 안전한지 조금 알아보았다.

# @Data
- @Data는 `@ToString`, `@EqualsAndHashCode`, `@Getter`, `@Setter`, `@RequireArgsConstructor` 를 제공한다.

### @Setter
- Setter는 객체가 언제든지 변경될 가능성을 제공하기 때문에 사용을 지양하여야 한다.

### @ToString
- 1:N 양방향 연관관계가 있는 경우 무한 순환 참조를 일으킬 수 있다. 그렇기 때문에 `@ToString(exclude = "Comment")` 처럼 특정 엔티티를 제외시키는 방법으로 사용하여야 한다.


# 생성자 생성 어노테이션
### @NoArgsConstructor
- 기본 생성자를 직접 생성하거나 @NoArgsConstructor를 붙이면 기본적으로 접근자가 public 으로 생성이 된다. 이 경우 객체 생성시 안전성이 떨어지고 private일 경우는 JPA 가 프록시를 만들 때 접근하지 못하기 때문에 protected 로 열어두길 권장하고 있다.
`@NoArgsConstructor(access = AccessLevel.Protected)`

### @AllArgsConstructor, @RequiredArgsConstructor
- @AllArgsConstructor는 모든 필드값을 파라미터로 받는 생성자를 직접 만들어주는 생성자이고 @RequiredArgsConstructor는 final 키워드가 붙은 필드의 생성자를 만들어준다.
- 둘은 모두 해당 클래스의 필드 순서에 따라 파라미터가 생성된다.

```java
@AllArgsConstructor
public class Animal {
    private String cat;
    private String dog;
}
```
- Animal 이라는 객체가 있고 cat과 dog라는 필드를 가지고 있다.
- 그러나 개를 더 좋아하는 개발자가 cat과 dog의 순서를 바꾸었다고 가정한다면, IDE가 제공하는 리팩토링이 동작하지 않고 lombok도 변화를 알아차리지 못한다.

```java
@AllArgsConstructor
public class Animal {
    private String dog;
    private String cat;
}

Animal animal = New Animal("멍멍", "야옹");
```

- 따라서 New Animal("멍멍", "야옹"); 이라고 인스턴스를 만든다면 cat이 멍멍하고 dog가 야옹하게 된다.
- 이 경우 타입마저 동일하기 때문에 에러없이 동작하게 된다는 점도 무서운 일이다.
- 따라서 직접 생성자를 만들고 그 생성자에 @Builder를 붙여서 사용하는 것이 권장된다. 파라미터 순서와 상관없이 이름으로 값을 설정하기 때문에 위와 같은 오류를 방지할 수 있다.

```java
public class Animal {
    private String dog;
    private String cat;

	@Builder
	private Animal(String dog, String cat) {
		this.dog = dog;
		this.cat = cat;
	}
}
```

```java
Animal animal = Animal.Builder()
		.dog("멍멍")
		.cat("야옹")
		.build();
```


---
### 관련 자료
- [자주 사용되는 Lombok, 주의사항](https://devk0ng.github.io/2021/07/30/lombok/#AllArgsConstructor)
- [올바른 Lombok 사용법](https://www.nowwatersblog.com/springboot/springstudy/lombok)