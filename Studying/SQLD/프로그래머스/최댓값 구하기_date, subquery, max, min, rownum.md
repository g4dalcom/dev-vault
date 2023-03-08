# 문제링크(https://school.programmers.co.kr/learn/courses/30/lessons/59415)

## 📝 문제

가장 최근에 들어온 동물은 언제 들어왔는지 조회하는 SQL 문을 작성해주세요.

---

### 🔍 정답

```sql
SELECT DATETIME AS 시간
FROM (SELECT DATETIME FROM ANIMAL_INS ORDER BY DATETIME DESC)
WHERE ROWNUM <= 1
```

### 🔍 정답

```sql
SELECT MAX(DATETIME) AS 시간
FROM ANIMAL_INS
```
- 시간(DATE)는 최댓값이 가장 최근 시간을 반환!