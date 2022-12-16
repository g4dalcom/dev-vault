# Spring Rest Docs?

### Swagger VS REST Docs

- API 문서 자동화를 위한 툴

##### Swagger
- `OpenAPI Spec`에 맞게 디자인 & 문서화하고 빌드하기 위한 도구 모음
- RESTful 웹 서비스를 설명, 생성, 시각화하기 위한 인터페이스
- yml 이나 json 파일 형식
- 특징
	- Annotation 기반으로 문서 생성(Production 코드에 침입)
	- 적용 난이도가 낮음(검증되지 않은 API 문서 생성)
	- 화면에서 API 테스트 가능

##### Rest Docs
- 테스트 코드 기반으로 `snippet` 을 생성해 RESTful 문서 생성을 돕는 도구
	- snippet : 문서화에 필요한 문서 조각
- `Asciidoctor`를 사용하여 HTML 생성
	- .adoc 파일을 HTML, DocBook 등으로 변환하기 위한 텍스트 프로세서
- 특징
	- 테스트 기반이라 API 신뢰성 보장, API 명세 최신화가 강제됨(적용 난이도 높음)
	- Production 코드에 영향이 없음(but 테스트 코드 양 증가)

> Rest Docs를 openAPI Spec으로 변환 후 Swagger UI에서 사용하는 방법도 있다!


# Spring Rest Docs 설정

### 개발환경
- Java 11
- Spring Boot 2.7.7
- gradle 7.5.1

## build.gradle

##### plugins
- Asciidoctor 플러그인 적용
	- 아스키닥터 파일을 컨버팅하고 build 폴더에 복사
- gradle 7.0 이상부터는 jvm 사용
```java
plugins {
    id 'org.asciidoctor.jvm.convert' version '3.3.2'
}
```

##### configurations
- dependencies에서 적용한 것 추가
```java
configurations {  
    asciidoctorExtensions  
}
```

##### dependencies
- restdocs-mockmvc의 testCompile 구성 -> mockMvc를 사용해서 snippets 조각들을 뽑아낼 수 있게 된다.
- MockMvc 대신 WebTestClient을 사용하려면 spring-restdocs-webtestclient 추가
- MockMvc 대신 REST Assured를 사용하려면 spring-restdocs-restassured 를 추가
```java
testImplementation 'org.springframework.restdocs:spring-restdocs-mockmvc'
asciidoctorExtensions 'org.springframework.restdocs:spring-restdocs-asciidoctor'
```

##### dependencies 아래에 설정
- asciidoctor 작업구성
	- `inputs.dir snippetsDir` : snippetsDir을 입력으로 구성
	- `dependsOn test` : 테스트 작업 이후 동작
	- `sources` :  (선택)
		- source가 없으면 .adoc파일을 전부 html로 만들어버림
		- source 지정시 특정 adoc만 HTML로 만든다.
	- `baseDirFollowsSourceFile()` : (선택)
		- 특정 .adoc에 다른 adoc 파일을 가져와서(include) 사용하고 싶을 경우 경로를 baseDir로 맞춰주는 설정으로, 개별 adoc으로 운영한다면 필요 없는 옵션
```java
// 변수 선언
ext {  
    snippetsDir = file('build/generated-snippets')  
}

// 위에서 작성한 snippetsDir 디렉토리를 test의 output으로 구성하는 설정 -> 스니펫 조각들이 build/generated-snippets로 출력
tasks.named('test') {  
    outputs.dir snippetsDir  
    useJUnitPlatform()  
}

// asciidoctor 작업구성
asciidoctor {  
    configurations 'asciidoctorExtensions'
    baseDirFollowsSourceFile()
    inputs.dir snippetsDir
    dependsOn test

    sources{
        include("**/index.adoc","**/common/*.adoc")
    }   
}  

// static/docs 폴더 비우기(기존에 사용되었던 파일 재사용 방지)
asciidoctor.doFirst {  
    delete file('src/main/resources/static/docs')  
}  

// asciidoctor 작업 이후 생성된 HTML파일을 static/docs에 copy
task copyDocument(type: Copy) {  
    dependsOn asciidoctor  
  
    from file("build/docs/asciidoc")  
    into file("src/main/resources/static/docs")  
}  
  
bootJar {  
    dependsOn copyDocument  
    from("${asciidoctor.ouputDir}")  
    into "static/docs"  
}

```

##### (선택) Snippet을 가독성 있게 출력
###### RestDocsConfiguration
```java
@TestConfiguration  
public class RestDocsConfiguration {  
  
    @Bean  
    public RestDocumentationResultHandler write() {  
        return MockMvcRestDocumentation.document(  
				/**
                 * 스니펫이 생성되는 디렉토리명
                 * generated-snippets 아래에 클래스명 / 메소드명 안에 스니펫이 생성된다.
                 */
                "{class-name}/{method-name}",  
                Preprocessors.preprocessRequest(Preprocessors.prettyPrint()),  
                Preprocessors.preprocessResponse(Preprocessors.prettyPrint())  
        );  
    }  
}
```
- test 폴더에 configuration을 만들어서 Bean으로 등록해준다.

# TEST

### Member
```java
@Entity  
@Getter  
public class Member {  
  
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)  
    private Long id;  
  
    @Column(nullable = false, unique = true)  
    private String email;  
  
    @Column(nullable = false)  
    private String name;  
  
    protected Member() {  
    }  
  
    @Builder  
    public Member(Long id, String email, String name) {  
        this.id = id;  
        this.email = email;  
        this.name = name;  
    }  
}
```

### MemberController
```java
@RestController
@RequiredArgsConstructor
public class MemberController {

    @GetMapping("/api/members/{id}")
    public ResponseEntity<Member> getMember(@PathVariable Long id) {
        return ResponseEntity.ok().body(Member.builder()
                .id(id)
                .build());
    }
}
```
- 테스트 코드 작성시, responseFields를 사용하려면 객체를 반환해야 함을 주의하자

### MemberControllerTest
```java
@SpringBootTest
@AutoConfigureMockMvc
@AutoConfigureRestDocs
@ExtendWith(MockitoExtension.class)
class MemberControllerTest {
    @Autowired
    MockMvc mockMvc;

    @Mock
    private MemberRepository memberRepository;

    @Test
    public void getMember() throws Exception {
        Member member1 = Member.builder()
                .id(1L)
                .email("user1@email.com")
                .name("user1")
                .build();

        lenient().when(memberRepository.findById(anyLong())).thenReturn(Optional.of(member1));

        mockMvc.perform(get("/api/members/{id}", member1.getId())
                .accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk())
                .andDo(document("get-member",
                        pathParameters(
                                parameterWithName("id").description("Member ID")
                        ),
                        responseFields(
                                fieldWithPath("id").description("아이디"),
                                fieldWithPath("name").description("이름"),
                                fieldWithPath("email").description("이메일")
                            )
                ));
    }
}
```

##### 테스트 성공!
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fd8itct%2FbtrTR0ZDuS2%2Fh2zdrmQ40HW7jsGvwVhkf0%2Fimg.png)
- 테스트가 성공하면 .adoc 파일이 생성되고
- 인텔리제이 플러그인 `Asciidoc` 을 설치하면 미리보기도 가능하다.

# 생성된 스니펫 사용하기

- 생성된 스니펫을 문서화하려면 .adoc 소스 파일을 생성해야 하고 그 소스 파일명과 동일한 html 문서가 생성된다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F2Sh4g%2FbtrTP4I5T0p%2FfdkFr2smnt6IFz3ah94lb1%2Fimg.png)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F5dgaX%2FbtrTRCraxi4%2FjxfAe7TfiCXj90ckIivWHk%2Fimg.png)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FG1TIT%2FbtrTRB6Qx02%2FMygFCYRpsirL8Lpf1TAAu1%2Fimg.png)
- src/docs/asciidoc 폴더를 생성한 후 위와 같이 .adoc 파일을 만들어야 한다.
- index.adoc은 다른 .adoc 문서들을 모아둔 곳이라고 보면 된다!
- asciidoc 작성 문법이 몇 가지 있는데 [Asciidoctor 공식문서](https://asciidoctor.org/docs/) 혹은 [유튜브_승팡, 케이의 REST docs](https://www.youtube.com/watch?v=BoVpTSsTuVQ&t=720s) 을 참고하도록 하자^^

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FOvApo%2FbtrTRfiHzjQ%2Fpl81IC8CIf8tYXz4kmazF1%2Fimg.png)
- 그리고 빌드를 하게 되면 static/docs 폴더에 .adoc 소스파일과 동일한 이름의 html이 생성된다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcYVkV8%2FbtrTR8wrT2C%2FYOM5EIBwzE5kmPXswFunkK%2Fimg.png)
- 스프링부트를 구동하고 http://localhost:8080/doc/index.html 로 이동하면 API 문서가 만들어진 것을 볼 수 있다!

---
### 관련 자료

- [Asciidoctor 공식문서](https://asciidoctor.org/docs/)
- [유튜브_승팡, 케이의 REST docs](https://www.youtube.com/watch?v=BoVpTSsTuVQ&t=720s)
- [Tecoble_API 문서 자동화 - Spring REST Docs 팔아보겠습니다](https://tecoble.techcourse.co.kr/post/2020-08-18-spring-rest-docs/)
- [REST-DOCS 적용하기](https://gaemi606.tistory.com/entry/Spring-Boot-REST-Docs-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0)
- [Spring Rest Docs 적용 및 최적화](https://backtony.github.io/spring/2021-10-15-spring-test-3/)
- [API 문서 자동화](https://tecoble.techcourse.co.kr/post/2020-08-18-spring-rest-docs/)
- [content as it could not be parsed as JSON or XML 오류 관련](https://lannstark.tistory.com/10)