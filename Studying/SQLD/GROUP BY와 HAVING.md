# GROUP BY

- GROUP BY는 데이터들을 원하는 그룹으로 나눌 수 있다.
- 그룹화하기 위해 집계 함수(COUNT, MAX, MIN, SUM, AVG)와 자주 사용된다!

```SQL
SELECT COUNTRY, COUNT(COUNTRY) AS COUNRY_COUNT
FROM CUSTOMERS
GROUP BY COUNTRY
ORDER BY COUNTRY
```
- CUSTOMERS 테이블에서 나라별 고객의 수

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcCMC4i%2Fbtr22I4Dky4%2FuosrVAjpRLpYi44SM7dMS1%2Fimg.png)

# HAVING

- GROUP BY 절에 의해 생성된 결과 값 중 원하는 조건의 데이터를 분류하기 위해 사용(WHERE과 유사하지만 GROUP BY와 사용된다는 점에 차이가 있다!)

```SQL
SELECT COUNTRY, COUNT(COUNTRY) AS COUNRY_COUNT
FROM CUSTOMERS
GROUP BY COUNTRY
HAVING COUNT(COUNTRY) >= 10
```
- 나라별 고객의 수가 10 이상인 경우만 출력

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FndKlo%2Fbtr21DWPTb2%2FSTPkBxKP2j78l4LXp6H1yK%2Fimg.png)
