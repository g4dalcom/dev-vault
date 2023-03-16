# SQL 기본

## SQL 문장의 종류

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlKyYe%2Fbtr37BcyKIt%2FtdDE4FxdAXK7f2l7mwCPR0%2Fimg.png)


# SQL 심화

## 그룹 함수
- 소계를 구할 때 사용

### ROLLUP
- 컬럼의 순서가 수행결과에 영향을 미침
- 인자로 들어온 컬럼의 오른쪽부터 하나씩 빼면서 그룹을 만듦

```sql
GROUP BY ROLLUP(COL1, COL2, COL3)

GROUP BY 결과 1 : COL1, COL2, COL3
GROUP BY 결과 2 : COL1, COL2
GROUP BY 결과 3 : COL1
GROUP BY 결과 4 : () -> 전체합계
```

```SQL
GROUP BY ROLLUP(COL1, (COL2, COL3))

GROUP BY 결과 1 : COL1, (COL2, COL3)
GROUP BY 결과 2 : COL1
GROUP BY 결과 3 : () -> 전체합계
```

```SQL
GROUP BY COL1, ROLLUP((COL2, COL3))

GROUP BY 결과 1 : COL1, (COL2, COL3)
GROUP BY 결과 2 : COL1
```


### CUBE
- 인자로 주어진 컬럼의 결합 가능한 모든 조합
- 컬럼의 순서가 수행결과와 상관없음

```SQL
GROUP BY CUBE(COL1, COL2)

GROUP BY 결과 1 : COL1, COL2
GROUP BY 결과 2 : COL1
GROUP BY 결과 3 : COL2
GROUP BY 결과 4 : () -> 전체합계
```

```SQL
GROUP BY CUBE(COL1)

GROUP BY 결과 1 : COL1
GROUP BY 결과 2 : () -> 전체합계
```


### GROUPING SETS
- 원하는 컬럼만 지정해서 소계를 구함
- UNION ALL 과 결과가 동일

```SQL
GROUP BY GROUPING SETS(COL1, COL2)

GROUP BY 결과 1 : COL1
GROUP BY 결과 2 : COL2
```

```SQL
GROUP BY GROUPING SETES((COL1, COL2), COL2, ())

GROUP BY 결과 1 : (COL1, COL2)
GROUP BY 결과 2 : COL2
GROUP BY 결과 3 : ()
```


#### GROUPING 함수
- ROLLUP, CUBE, GROUPING SETS 함수와 함께 쓰이며 GROUP BY에서 쓰인 소계 함수 결과 CASE에서 빠진 컬럼에 대해 1을 반환

```SQL
GROUP BY ROLLUP(COL1, COL2, COL3)

GROUP BY 결과 1 : COL1, COL2, COL3
GROUP BY 결과 2 : COL1, COL2, [NULL] -> GROUPING (COL3) 의 결과는 1이 된다.
GROUP BY 결과 3 : COL1, [NULL], [NULL]
GROUP BY 결과 4 : () -> 전체합계
```


## 윈도우 함수
- SELECT 결과에 대하여 `행과 행간의 관계를 실시간으로 파악`
- SELECT 결과에 대한 관계의 파악이므로 행수의 변화는 없음

- `윈도우함수() OVER (PARTITION COLUMN ORDER BY COLUMN ASC/DESC)`
- 대상 행을 지정하는 구문이 올 수도 있음
	- `RANGE BETWEEN A AND B` 
	- DEFAULT : `RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`
		- UNBOUNDED PRECEDING : 최종 출력될 값의 맨 처음 ROW값
		- CURRENT ROW : 현재 ROW 값
		- UNBOUNDED FOLLOWING : 최종 출력될 값의 맨 마지막 ROW값(AND 뒤)

1. 윈도우 함수() 
	- 순위함수
		- ROW_NUMBER(동일 값도 고유한 순위 부여), 1, 2, 3, 4, 5
		- RANK(동일 값 동일 순위, 중간 순위 비움), 1, 2, 2, 4, 4, 6
		- DENSE_RANK(동일 값 동일 순위, 하나의 건수), 1, 2, 2, 3, 3, 4
	-  집계함수
		- COUNT / SUM / MAX / MIN / AVG
	- 행순서함수
		- LAG(이전 값)
		- LEAD(다음 값)
		- FIRST_VALUE(가장 처음에 나온 값)
		- LAST_VALUE(가장 나중에 나온 값)
	- 비율함수
		- RATIO_TO_REPORT / CUM_DIST / NTILE / PERCENT_RANK
1. OVER : 윈도우 함수 필수 요건, OVER 내부에 PARTITION BY절과 ORDER BY절이 옴
2. PARTITION BY : 전체 집합을 어떤 기준(컬럼)에 따라 나눌지 결정
3. ORDER BY : 어떤 항목(컬럼)을 기준으로 순위를 정할지 결정


## 권한

1.  GRANT : 권한 허용
	- WITH GRANT OPTION : 권한을 받은 유저가 동일 권한을 줄 수 있는 옵션
2. DENY : 유저에게 개체에 대한 권한을 차단
3. REVOKE : 권한 회수
	- CASCADE : WITH GRANT OPTION으로 부여된 권한까지 회수

```SQL
GRANT SELECT ON SCHEMA::A_USER TO 유저명;
REVOKE UPDATE ON SCHEMA::A_USER FROM 유저명;
DENY DELETE ON SCHEMA::A_USER TO 유저명;
DENY SELECT ON A_USER.TABLE1 TO 유저명;
```