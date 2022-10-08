---
keyword : Spring, Querydsl
class : Programming
---

# 사용자 정의 리포지토리
## 사용자 정의 리포지토리 사용법
1. 사용자 정의 인터페이스 작성
2. 사용자 정의 인터페이스 구현
3. 스프링 데이터 리포지토리에 사용자 정의 인터페이스 상속

## 사용자 정의 리포지토리 구성
###### 1. 사용자 정의 인터페이스 작성
```java
package study.querydsl.repository;
import study.querydsl.dto.MemberSearchCondition;
import study.querydsl.dto.MemberTeamDto;
import java.util.List;
public interface MemberRepositoryCustom {
 List<MemberTeamDto> search(MemberSearchCondition condition);
}
```


###### 2. 사용자 정의 인터페이스 구현
```java
package study.querydsl.repository;
import com.querydsl.core.types.dsl.BooleanExpression;
import com.querydsl.jpa.impl.JPAQueryFactory;
import study.querydsl.dto.MemberSearchCondition;
import study.querydsl.dto.MemberTeamDto;
import study.querydsl.dto.QMemberTeamDto;
import javax.persistence.EntityManager;
import java.util.List;
import static org.springframework.util.StringUtils.isEmpty;
import static study.querydsl.entity.QMember.member;
import static study.querydsl.entity.QTeam.team;
public class MemberRepositoryImpl implements MemberRepositoryCustom {
 private final JPAQueryFactory queryFactory;
 public MemberRepositoryImpl(EntityManager em) {
 this.queryFactory = new JPAQueryFactory(em);
 }
 @Override
 //회원명, 팀명, 나이(ageGoe, ageLoe)
 public List<MemberTeamDto> search(MemberSearchCondition condition) {
 return queryFactory
 .select(new QMemberTeamDto(
 member.id,
 member.username,
 member.age,
 team.id,
 team.name))
 .from(member)
 .leftJoin(member.team, team)
 .where(usernameEq(condition.getUsername()),
 teamNameEq(condition.getTeamName()),
 ageGoe(condition.getAgeGoe()),
 ageLoe(condition.getAgeLoe()))
 .fetch();
 }
 private BooleanExpression usernameEq(String username) {
 return isEmpty(username) ? null : member.username.eq(username);
 }
 private BooleanExpression teamNameEq(String teamName) {
 return isEmpty(teamName) ? null : team.name.eq(teamName);
 }
 private BooleanExpression ageGoe(Integer ageGoe) {
 return ageGoe == null ? null : member.age.goe(ageGoe);
 }
 private BooleanExpression ageLoe(Integer ageLoe) {
 return ageLoe == null ? null : member.age.loe(ageLoe);
 }
}
```

###### 3. 스프링 데이터 리포지토리에 사용자 정의 인터페이스 상속
```java
package study.querydsl.repository;
import org.springframework.data.jpa.repository.JpaRepository;
import study.querydsl.entity.Member;
import java.util.List;
public interface MemberRepository extends JpaRepository<Member, Long>,
MemberRepositoryCustom {
 List<Member> findByUsername(String username);
}
```

###### 커스텀 리포지토리 동작 테스트 추가
```java
@Test
public void searchTest() {
 Team teamA = new Team("teamA");
 Team teamB = new Team("teamB");
 em.persist(teamA);
 em.persist(teamB);
 Member member1 = new Member("member1", 10, teamA);
 Member member2 = new Member("member2", 20, teamA);
 Member member3 = new Member("member3", 30, teamB);
 Member member4 = new Member("member4", 40, teamB);
 em.persist(member1);
 em.persist(member2);
 em.persist(member3);
 em.persist(member4);
 MemberSearchCondition condition = new MemberSearchCondition();
 condition.setAgeGoe(35);
 condition.setAgeLoe(40);
 condition.setTeamName("teamB");
 List<MemberTeamDto> result = memberRepository.search(condition);
 assertThat(result).extracting("username").containsExactly("member4");
}
```


## 스프링 데이터 페이징 활용1 - Querydsl 페이징 연동

###### 사용자 정의 인터페이스에 페이징 2가지 추가
```java
package study.querydsl.repository;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import study.querydsl.dto.MemberSearchCondition;
import study.querydsl.dto.MemberTeamDto;
import java.util.List;
public interface MemberRepositoryCustom {
 List<MemberTeamDto> search(MemberSearchCondition condition);
 Page<MemberTeamDto> searchPageSimple(MemberSearchCondition condition,
Pageable pageable);
 Page<MemberTeamDto> searchPageComplex(MemberSearchCondition condition,
Pageable pageable);
}
```

###### 전체 카운트를 한번에 조회하는 단순한 방법
- searchPageSimple(), fetchResults() 사용
```java
/**
 * 단순한 페이징, fetchResults() 사용
 */
@Override
public Page<MemberTeamDto> searchPageSimple(MemberSearchCondition condition,
Pageable pageable) {
	 QueryResults<MemberTeamDto> results = queryFactory
		 .select(new QMemberTeamDto(
				 member.id,
				 member.username,
				 member.age,
				 team.id,
				 team.name))
		 .from(member)
		 .leftJoin(member.team, team)
		 .where(usernameEq(condition.getUsername()),
				 teamNameEq(condition.getTeamName()),
				 ageGoe(condition.getAgeGoe()),
				 ageLoe(condition.getAgeLoe()))
		 .offset(pageable.getOffset())
		 .limit(pageable.getPageSize())
		 .fetchResults();
		 
	 List<MemberTeamDto> content = results.getResults();
	 
	 long total = results.getTotal();
	 
	 return new PageImpl<>(content, pageable, total);
}
```
- Querydsl이 제공하는 fetchResults() 를 사용하면 내용과 전체 카운트를 한번에 조회할 수 있다.(실제 쿼리는 2번 호출)
- fetchResult() 는 카운트 쿼리 실행시 필요없는 order by 는 제거한다.

###### 데이터 내용과 전체 카운트를 별도로 조회하는 방법
searchPageComplex(
```java
import org.springframework.data.support.PageableExecutionUtils; //패키지 변경

public Page<MemberTeamDto> searchPageComplex(MemberSearchCondition condition,
Pageable pageable) {
	 List<MemberTeamDto> content = queryFactory
		 .select(new QMemberTeamDto(
				 member.id.as("memberId"),
				 member.username,
				 member.age,
				 team.id.as("teamId"),
				 team.name.as("teamName")))
		 .from(member)
		 .leftJoin(member.team, team)
		 .where(
				 usernameEq(condition.getUsername()),
				 teamNameEq(condition.getTeamName()),
				 ageGoe(condition.getAgeGoe()),
				 ageLoe(condition.getAgeLoe())
				 )
		 .offset(pageable.getOffset())
		 .limit(pageable.getPageSize())
		 .fetch();
		 
		 JPAQuery<Long> countQuery = queryFactory
			 .select(member.count())
			 .from(member)
			 .leftJoin(member.team, team)
			 .where(
					 usernameEq(condition.getUsername()),
					 teamNameEq(condition.getTeamName()),
					 ageGoe(condition.getAgeGoe()),
					 ageLoe(condition.getAgeLoe())
		 );
		 
	 return PageableExecutionUtils.getPage(content, pageable,
	countQuery::fetchOne);
)
```

######  Querydsl fetchResults() , fetchCount() Deprecated(향후 미지원)
- Querydsl의 fetchCount() , fetchResult() 는 개발자가 작성한 select 쿼리를 기반으로 count용 쿼리를 내부에서 만들어서 실행합니다.
- 그런데 이 기능은 강의에서 설명드린 것 처럼 select 구문을 단순히 count 처리하는 용도로 바꾸는 정도입니다. 따라서 단순한 쿼리에서는 잘 동작하지만, 복잡한 쿼리에서는 제대로 동작하지 않습니다.
- Querydsl은 향후 fetchCount() , fetchResult() 를 지원하지 않기로 결정했습니다.
- 참고로 Querydsl의 변화가 빠르지는 않기 때문에 당장 해당 기능을 제거하지는 않을 것입니다.
- 따라서 count 쿼리가 필요하면 다음과 같이 별도로 작성해야 합니다.

###### count 쿼리는 예제
```java
@Test
public void count() {
 Long totalCount = queryFactory
		 //.select(Wildcard.count) //select count(*)
		 .select(member.count()) //select count(member.id)
		 .from(member)
		 .fetchOne();
 System.out.println("totalCount = " + totalCount);
}
```
- count(*) 을 사용하고 싶으면 예제의 주석처럼 Wildcard.count 를 사용하시면 됩니다.
- member.count() 를 사용하면 count(member.id) 로 처리됩니다.
- 응답 결과는 숫자 하나이므로 fetchOne() 을 사용합니다

## 스프링 데이터 페이징 활용2 - CountQuery 최적화
###### PageableExecutionUtils.getPage()로 최적화
```java
JPAQuery<Member> countQuery = queryFactory
	 .select(member)
	 .from(member)
	 .leftJoin(member.team, team)
	 .where(usernameEq(condition.getUsername()),
			 teamNameEq(condition.getTeamName()),
			 ageGoe(condition.getAgeGoe()),
			 ageLoe(condition.getAgeLoe()));
			 
	// return new PageImpl<>(content, pageable, total);
	 return PageableExecutionUtils.getPage(content, pageable,
	countQuery::fetchCount);
```
 - 스프링 데이터 라이브러리가 제공
- count 쿼리가 생략 가능한 경우 생략해서 처리
- 페이지 시작이면서 컨텐츠 사이즈가 페이지 사이즈보다 작을 때
- 마지막 페이지 일 때 (offset + 컨텐츠 사이즈를 더해서 전체 사이즈 구함)

## 스프링 데이터 페이징 활용3 - 컨트롤러 개발
###### 실제 컨트롤러
```java
package study.querydsl.controller;
import lombok.RequiredArgsConstructor;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import study.querydsl.dto.MemberSearchCondition;
import study.querydsl.dto.MemberTeamDto;
import study.querydsl.repository.MemberJpaRepository;
import study.querydsl.repository.MemberRepository;
import java.util.List;

@RestController
@RequiredArgsConstructor
public class MemberController {
	 private final MemberJpaRepository memberJpaRepository;
	 private final MemberRepository memberRepository;
	 
	 @GetMapping("/v1/members")
	 public List<MemberTeamDto> searchMemberV1(MemberSearchCondition condition)
	{
		 return memberJpaRepository.search(condition);
	 }
	 
	 @GetMapping("/v2/members")
	 public Page<MemberTeamDto> searchMemberV2(MemberSearchCondition condition,
	Pageable pageable) {
		 return memberRepository.searchPageSimple(condition, pageable);
	 }
	 
	 @GetMapping("/v3/members")
	 public Page<MemberTeamDto> searchMemberV3(MemberSearchCondition condition,
	Pageable pageable) {
		 return memberRepository.searchPageComplex(condition, pageable);
	 }
}
```
- http://localhost:8080/v2/members?size=5&page=2

## 스프링 데이터 정렬(Sort)
- 스프링 데이터 JPA는 자신의 정렬(Sort)을 Querydsl의 정렬(OrderSpecifier)로 편리하게 변경하는 기능을 제공한다. 이 부분은 뒤에 스프링 데이터 JPA가 제공하는 Querydsl 기능에서 살펴보겠다.
- 스프링 데이터의 정렬을 Querydsl의 정렬로 직접 전환하는 방법은 다음 코드를 참고하자.

###### 스프링 데이터 Sort를 Querydsl의 OrderSpecifier로 변환
```java
JPAQuery<Member> query = queryFactory
	 .selectFrom(member);
	 
for (Sort.Order o : pageable.getSort()) {
	 PathBuilder pathBuilder = 
			 new PathBuilder(member.getType(),
member.getMetadata());
	 query.orderBy(new OrderSpecifier(o.isAscending() ? Order.ASC : Order.DESC,
		 pathBuilder.get(o.getProperty())));
}

	List<Member> result = query.fetch();
```
-  참고: 정렬( Sort )은 조건이 조금만 복잡해져도 Pageable 의 Sort 기능을 사용하기 어렵다. 루트 엔티티 범위를 넘어가는 동적 정렬 기능이 필요하면 스프링 데이터 페이징이 제공하는 Sort 를 사용하기 보다는 파라미터를 받아서 직접 처리하는 것을 권장한다.

