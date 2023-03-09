# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/59405)

## 📝 문제

동물 보호소에 가장 먼저 들어온 동물의 이름을 조회하는 SQL 문을 작성해주세요.

---

### 🔍 정답

```sql
SELECT NAME
FROM (SELECT * FROM ANIMAL_INS ORDER BY DATETIME)
WHERE ROWNUM = 1
```
- ORDER BY 는 가장 마지막에 연산하기 때문에 서브쿼리로 FROM절에서 먼저 정렬을 해주고 ROWNUM 을 통해 가장 상위 하나만 불러온다!