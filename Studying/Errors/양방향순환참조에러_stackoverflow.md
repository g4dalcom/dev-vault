---
keyword : Spring
class : ERROR
---


# 양방향순환참조에러, Stackoverflow: null


#### 에러내용

- 미니프로젝트 중 comment를 등록하면 로그에 stackoverflow: null이라는 메시지와 함께 무한 로그가 출력되며 서버가 터졌다.
- 로그에는 post["comment"] -> comment -> member -> post["comment"] 이런식으로 찍혀있어 무한 순환참조가 일어나는 듯 했다.

---

#### 원인

- JPA에서 양방향으로 연결된 entity를 JSON형태로 직렬화하는 과정에서 서로의 정보를 계속해서 순환하여 참조하여 stackoverflow가 발생한 것이다.
- Spring Boot는 @ResponseBody(rest api)를 구현할 시 Object를 JSON 형태로 변환하기 위해 Jackson 라이브러리를 이용하는데, Jackson은 entity의 getter를 호출하고, 직렬화를 이용해 JSON 형태로 객체를 변화시키고 view로 전달한다.
- getter를 호출하는 과정에서부터 순환 참조가 계속 발생해 view로 전달하면서 stackoverflow가 발생하게 된다.

- ※ 직렬화란, 객체의 내용을 바이트 단위로 변환하여 파일 또는 네트워크를 통해 스트림(송수신)하도록 하는 것을 의미한다.


다음 코드는 게시글과 댓글의 Entity 클래스이다.

```java
@NoArgsConstructor
@AllArgsConstructor
@Builder
@Getter
@Entity
public class Posts extends TimeEntity {     
	@Id   
	@GeneratedValue(strategy = GenerationType.IDENTITY)    
	private Long id;
	
	...        
	
	@ManyToOne(fetch = FetchType.LAZY)    
	@JoinColumn(name = "user_id")    
	private User user;     
	
	@OneToMany(mappedBy = "posts", fetch = FetchType.EAGER, cascade = CascadeType.REMOVE)    
	private List<Comment> comments;     
	
	...
	
}

```

```java
@Builder
@AllArgsConstructor
@NoArgsConstructor
@Getter
@Table(name = "comments")
@Entitypublic 
class Comment {     
	@Id    
	@GeneratedValue(strategy = GenerationType.IDENTITY)    
	private Long id;
	
	...     
	
	@ManyToOne    
	@JoinColumn(name = "posts_id")    
	private Posts posts;     
	
	@ManyToOne    
	@JoinColumn(name = "user_id")    
	private User user; // 작성자}

```

Posts Entity에서 Comment를 부르는 부분을 보면 다음과 같다.

```java
@OneToMany(mappedBy = "posts", fetch = FetchType.EAGER, cascade = CascadeType.REMOVE)
private List<Comment> comments;
```

Posts에서 Comment는 연관관계의 주인이 아니기 때문에 데이터베이스에서 따로 FK로 저장되진 않는다.

하지만 JPA는 서버에서 클라이언트로 Posts의 정보를 보내줄 때 댓글 정보도 같이 보내주게 된다.

반대로, Comment Entity에서 Posts를 부르는 부분을 보자.

```java
@ManyToOne
@JoinColumn(name = "posts_id")
private Posts posts;
```

Comment는 연관관계의 주인이므로 데이터베이스에 Posts에 대한 "posts_id"를 FK로 갖고 있다.

이 또한 JPA가 서버에서 클라이언트로 Comment의 정보를 보내줄 때 Posts 정보도 같이 보내주게 된다.

REST API로  Entity를 컨트롤러에서 직접조회하는 경우 다음과 같은 결과를 초래한다.

![](https://blog.kakaocdn.net/dn/cIeWBC/btrpUImKstK/CAFyuY5lxx105Ogj3QomQ0/img.png)

Posts > Comment > Posts > Comment와 같이 서로를 계속해서 참조하게되어 순환 참조를 하는 것이다.


---

#### 해결

순환 참조를 방지하기 위한 방법은 여러 가지가 있다.

##### 1. @JsonIgnore

이 어노테이션을 붙이면 JSON 데이터에 해당 프로퍼티는 null로 들어가게 된다. 
즉, 데이터에 아예 포함시키지 않는다.

##### 2. @JsonManagedReference 와 @JsonBackReference

부모 클래스(Posts entity)의 Comment 필드에 @JsonManagedReference를, 자식 클래스(Comment entity)의 Posts 필드에 @JsonBackReference를 추가해주면 순환 참조를 막을 수 있다.

##### 3.@JsonIgnoreProperties

부모 클래스(Posts entity)의 Comment 필드에 @JsonIgnoreProperties({"posts"}) 를 붙여주면 순환 참조를 막을 수 있다.

##### 4. DTO 사용

위와 같은 상황이 발생하게된 주원인은 '양방향 매핑'이기도 하지만, 더 정확하게는 Entity 자체를 response로 리턴한데에 있다. entity 자체를 return 하지 말고, DTO 객체를 만들어 필요한 데이터만 옮겨담아 Client로 리턴하면 순환 참조 관련 문제는 애초에 방지 할 수 있다.

##### 5. 매핑 재설정

양방향 매핑이 꼭 필요한지 다시 한번 생각해볼 필요가 있다. 만약 양쪽에서 접근할 필요가 없다면 단방향 매핑을 해줘서 자연스레 순환 참조 문제를 해결하자.

---

#### 참고문서

- [슬기로운 개발생활:티스토리](https://dev-coco.tistory.com/133) 
- [Blog-shine:티스토리](https://blogshine.tistory.com/436)
