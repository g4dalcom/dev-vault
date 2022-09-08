---
keyword : Spring
class : Programming
---



![](https://k.kakaocdn.net/dn/bRd0AX/btrqyrl6iKG/YDxRU5Vi2g0i1uoXjqxoKk/img.png)

위 DTO 클래스들을 간결한 코드로 만들며 개선한 내용을 정리해보겠다.

---

# DTO를 Inner Static Class로 관리

DTO 패키지 내 클래스 파일들을 깔끔하게 관리하기 위해 Inner Class(Nested Class)로 DTO를 관리하는 것이다.

```java
/**
 * request, response DTO 클래스를 하나로 묶어 InnerStaticClass로 한 번에 관리
 */
public class PostsDto {

    @Data
    @AllArgsConstructor
    @NoArgsConstructor
    @Builder
    public static class Request {
        private Long id;
        private String title;
        private String writer;
        private String content;
        private String createdDate, modifiedDate;
        private int view;
        private User user;
        
        /* Dto -> Entity */
        public Posts toEntity() {
            Posts posts = Posts.builder()
                    .id(id)
                    .title(title)
                    .writer(writer)
                    .content(content)
                    .view(0)
                    .user(user)
                    .build();
            return posts;
        }
    }
    @Getter
    public static class Response {
        private Long id;
        private String title;
        private String writer;
        private String content;
        private String createdDate, modifiedDate;
        private int view;
        private Long userId;
        private List<CommentDto.Response> comments;

        /* Entity -> Dto*/
        public Response(Posts posts) {
            this.id = posts.getId();
            this.title = posts.getTitle();
            this.writer = posts.getWriter();
            this.content = posts.getContent();
            this.createdDate = posts.getCreatedDate();
            this.modifiedDate = posts.getModifiedDate();
            this.view = posts.getView();
            this.userId = posts.getUser().getId();
            this.comments = posts.getComments().stream().map(CommentDto.Response::new).collect(Collectors.toList());
        }
    }
}
```

### Controller

```java
@RequiredArgsConstructor
@RequestMapping("/api")
@RestController
public class PostsApiController {
    private final PostsService postsService;

    /* CREATE */
    @PostMapping("/posts")
    public ResponseEntity save(@RequestBody PostsDto.Request dto, @LoginUser UserDto.Response user) {
        return ResponseEntity.ok(postsService.save(dto, user.getNickname()));
    }

    /* READ */
    @GetMapping("/posts/{id}")
    public ResponseEntity read(@PathVariable Long id) {
        return ResponseEntity.ok(postsService.findById(id));
    }

    /* UPDATE */
    @PutMapping("/posts/{id}")
    public ResponseEntity update(@PathVariable Long id, @RequestBody PostsDto.Request dto) {
        postsService.update(id, dto);
        return ResponseEntity.ok(id);
    }

    /* DELETE */
    @DeleteMapping("/posts/{id}")
    public ResponseEntity delete(@PathVariable Long id) {
        postsService.delete(id);
        return ResponseEntity.ok(id);
    }
}
```

### Service

```
@RequiredArgsConstructor
@Service
public class PostsService {

    private final PostsRepository postsRepository;
    private final UserRepository userRepository;

    @Transactional
    public Long save(PostsDto.Request dto, String nickname) {
        User user = userRepository.findByNickname(nickname);
        dto.setUser(user);
        Posts posts = dto.toEntity();
        postsRepository.save(posts);

        return posts.getId();
    }

    @Transactional(readOnly = true)
    public PostsDto.Response findById(Long id) {
        Posts posts = postsRepository.findById(id).orElseThrow(() ->
                new IllegalArgumentException("해당 게시글이 존재하지 않습니다. id: " + id));

        return new PostsDto.Response(posts);
    }

    @Transactional
    public void update(Long id, PostsDto.Request dto) {
        Posts posts = postsRepository.findById(id).orElseThrow(() ->
                new IllegalArgumentException("해당 게시글이 존재하지 않습니다. id: " + id));

        posts.update(dto.getTitle(), dto.getContent());
    }

    @Transactional
    public void delete(Long id) {
        Posts posts = postsRepository.findById(id).orElseThrow(() ->
                new IllegalArgumentException("해당 게시글이 존재하지 않습니다. id: " + id));

        postsRepository.delete(posts);
    }
    
    ...
}
```

---

![](https://k.kakaocdn.net/dn/dYLBqG/btrqGscbFHK/nemXrJhfvWIwRzRB7Avbb0/img.png)

이렇게 1개의 Class 파일로 DTO를 Inner Class로 관리하면 조금 더 깔끔하게 패키지를 관리할 수 있고,

코드의 캡슐화를 증가시키며, 코드의 복잡성도 줄일 수 있다는 장점이 생긴다.