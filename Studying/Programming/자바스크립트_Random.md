# Math.random()

- 0부터 1사이의 난수를 반환(1은 제외)

### min ~ max 사이 random 난수 구하기

```javascript
min + Math.random() * (max-min);

/* 1~5 사이의 난수 */
1 + Math.random() * (5-1);
```
- Math.random() = 0~1
- Math.random() * 4 = 0~4
- 1 + Math.random() * 4 = 1~5

### min ~ max 사이 random 정수 구하기

```javascript
Math.round(min - 0.5 + Math.random() * (max-min+1));

Math.floor(min + Math.random() * (max+1 - min))
```
- 1~3 사이 정수라면,
- 0.5~3.5 사이의 수를 반올림하거나
	- 0.5 ~ 1.49999999.. : 1
	- 1.5 ~ 2.4999999.. : 2
	- 2.5 ~ 3.4999999.. : 3
- 1~4 사이의 수를 내림하면 된다.
	- 1 ~ 1.9999999.. : 1
	- 2 ~ 2.9999999.. : 2
	- 3 ~ 3.9999999.. : 3

```javascript
/* 오답 */
Math.round(min + Math.random() * (max-min));
```
- 이 경우는 1~3 사이의 난수를 반올림하게 되는데 이렇게 되면 1, 3이 나올 확률보다 2가 나올 확률이 2배 정도 높아져서 랜덤수가 아니게 된다.
	- 1 ~ 1.4999999.. : 1
	- 1.5 ~ 2.499999.. : 2
	- 2.5 ~ 2.999999.. : 3
