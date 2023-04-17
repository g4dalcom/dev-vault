---
keyword : Java, Spring
class : Programming
---


### 강한 결합에서 느슨한 결합으로

👇 이렇게 되면 Repository 의 변경이 있을 때 모든 service와 controller를 변경해야 됨

```java
Public class Service {
	private final Repository repository;

	public Service {
		this.repository = new Repository();
	}
{
```

👇 Repository 에서 new 로 미리 생성한 객체를 그대로 이용

```java
Public class nameService {
	private final Repository repository;
	
	@Autowired
	public Service(Repository repository) {
		~~this.repository = new Repository();~~
		this.repository = repository;
	}
{
```

👇 갓롬복

```java
@RequiredArgsConstructor
Public class nameService {
	private final Repository repository;
	
	~~@Autowired
	public Service(Repository repository) {
		this.repository = new Repository();
		this.repository = repository;
	}~~
{
```