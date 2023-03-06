# 개요

- 오라클에서 if 문과 비슷한 기능을 하는 DECODE 함수가 있다. 그러나 DECODE 함수는 조건이 많아지면 가독성이 떨어지고 복잡해지며, 가장 큰 문제는 오라클 SQL에서만 사용할 수 있는 비표준 함수이다.
- DECODE를 대체할 수 있는 기능이 CASE 표현식이며 가독성이 좋고 더 많은 기능을 제공한다. 

# 사용 예제

- 일반적인 CASE 표현식
```sql
SELECT NAME,
       CITY_NO,
       CASE CITY_NO 
            WHEN 5 THEN 'FATEN'
            WHEN 10 THEN 'ELGACIA'
            ELSE 'Unknown'
       END AS WORLD
  FROM LOSTARK
 WHERE JOB = 'REAPER'
```

- WHERE 절에 사용
```SQL
SELECT NAME,
	   LEVEL,
	   CASE WHEN LEVEL >= 1415 THEN '발탄'
		    WHEN LEVEL >= 1475 THEN '쿠크세이튼'
		END AS ITEM_LEVEL
FROM LOSTARK
WHERE JOB = 'REAPER'
AND (CASE WHEN LEVEL >= 1415 THEN 1
		  WHEN LEVEL >= 1475 THEN 2
	  END) = 1
```

- 오라클 내장 함수를 조건으로 사용
```SQL
SELECT NAME,
	   HIREDATE,
	   CASE WHEN TO_CHAR(HIREDATE, 'q') = '1' THEN '1분기'
		    WHEN TO_CHAR(HIREDATE, 'q') = '2' THEN '2분기'
		END AS HIRE_QUARTER
FROM EMP
WHERE JOB = 'GUNSLINGER'
```

- THEN절에서 중첩 CASE 등 추가 연산 작업
```SQL
SELECT NAME,
	   LEVEL,
	   CITY_NO,
	   CASE WHEN CITY_NO = '10' THEN
				 CASE WHEN LEVEL >= 960 THEN '낙원의문'
					  WHEN LEVEL >= 1325 THEN '오레하'
			WHEN CITY_NO = '20' THEN
				 CASE WHEN LEVEL >= 1415 THEN '데스칼루다'
					  WHEN LEVEL >= 1460 THEN '쿤겔라니움'
			    END
		END AS ITEM_LEVEL
FROM LOSTARK
WHERE JOB = 'AEROMANCER'
```