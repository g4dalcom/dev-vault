---
keyword : Spring, Querydsl
class : Programming
---

## 실무 활용 - 순수 JPA와 Querydsl
#### 순수 JPA 리포지토리와 Querydsl
- 동적쿼리 Builder 적용
- 동적쿼리 Where 적용
- 조회 API 컨트롤러 개발
- 순수 JPA 리포지토리와 Querydsl

---

###### 순수 JPA 리포지토리

```java
package study.querydsl.repository;
import com.querydsl.core.BooleanBuilder;
import com.querydsl.core.types.dsl.BooleanExpression;
import com.querydsl.jpa.impl.JPAQueryFactory;
import org.springframework.stereotype.Repository;
import study.querydsl.dto.MemberSearchCondition;
import study.querydsl.dto.MemberTeamDto;
import study.querydsl.dto.QMemberTeamDto;
import study.querydsl.entity.Member;
import javax.persistence.EntityManager;
import java.util.List;
import java.util.Optional;
import static org.springframework.util.StringUtils.hasText;
import static org.springframework.util.StringUtils.isEmpty;
import static study.querydsl.entity.QMember.member;
import static study.querydsl.entity.QTeam.team;


@Repository
public class MemberJpaRepository {
	 private final EntityManager em;
	 private final JPAQueryFactory queryFactory;
	 
	 public MemberJpaRepository(EntityManager em) {
		 this.em = em;
		 this.queryFactory = new JPAQueryFactory(em);
	 }
	 
 public void save(Member member) {
	 em.persist(member);
 }
 
 public Optional<Member> findById(Long id) {
	 Member findMember = em.find(Member.class, id);
	 return Optional.ofNullable(findMember);
 }
 
 public List<Member> findAll() {
	 return em.createQuery("select m from Member m", Member.class)
	 .getResultList();
 }
 
 public List<Member> findByUsername(String username) {
	 return em.createQuery("select m from Member m where m.username 
	= :username", Member.class)
		 .setParameter("username", username)
		 .getResultList();
 }
}
```


###### 순수 JPA 리포지토리 테스트
```java
package study.querydsl.repository;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.transaction.annotation.Transactional;
import study.querydsl.dto.MemberSearchCondition;
import study.querydsl.dto.MemberTeamDto;
import study.querydsl.entity.Member;
import study.querydsl.entity.Team;
import javax.persistence.EntityManager;
import java.util.List;
import static org.assertj.core.api.Assertions.assertThat;


@SpringBootTest
@Transactional
class MemberJpaRepositoryTest {
 @Autowired
 EntityManager em;
 
 @Autowired
 MemberJpaRepository memberJpaRepository;
 
 @Test
 public void basicTest() {
	 Member member = new Member("member1", 10);
	 memberJpaRepository.save(member);
	 
	 Member findMember = memberJpaRepository.findById(member.getId()).get();
	 assertThat(findMember).isEqualTo(member);
	 
	 List<Member> result1 = memberJpaRepository.findAll();
	 assertThat(result1).containsExactly(member);
	 
	 List<Member> result2 = memberJpaRepository.findByUsername("member1");
	 assertThat(result2).containsExactly(member);
 }
```

###### Querydsl 사용
- 순수 JPA 리포지토리 - Querydsl 추가
```java
 public List<Member> findAll_Querydsl() {
	 return queryFactory
			 .selectFrom(member).fetch();
}

 public List<Member> findByUsername_Querydsl(String username) {
	 return queryFactory
			 .selectFrom(member)
			 .where(member.username.eq(username))
			 .fetch();
 }
```

- Querydsl 테스트 추가
```java
@Test
public void basicQuerydslTest() {
	 Member member = new Member("member1", 10);
	 memberJpaRepository.save(member);
	 
	 Member findMember = memberJpaRepository.findById(member.getId()).get();
 assertThat(findMember).isEqualTo(member);
 
	 List<Member> result1 = memberJpaRepository.findAll_Querydsl();
	 assertThat(result1).containsExactly(member);
	 
	 List<Member> result2 =
	memberJpaRepository.findByUsername_Querydsl("member1");
	 assertThat(result2).containsExactly(member);
}
```

- JPAQueryFactory 스프링 빈 등록
다음과 같이 JPAQueryFactory 를 스프링 빈으로 등록해서 주입받아 사용해도 된다.

```java
@Bean
JPAQueryFactory jpaQueryFactory(EntityManager em) {
	return new JPAQueryFactory(em);
}
```

- 참고: 동시성 문제는 걱정하지 않아도 된다. 왜냐하면 여기서 스프링이 주입해주는 엔티티 매니저는 실제 동작 시점에 진짜 엔티티 매니저를 찾아주는 프록시용 가짜 엔티티 매니저이다. 이 가짜 엔티티 매니저는 실제 사용 시점에 트랜잭션 단위로 실제 엔티티 매니저(영속성 컨텍스트)를 할당해준다.