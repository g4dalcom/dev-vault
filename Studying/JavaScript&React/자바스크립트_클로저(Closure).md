---
keyword : Javascript
class : Grammer
---

# 1. 개념

- 함수와 그 함수가 접근할 수 있는 변수의 조합

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbAeB6k%2Fbtr1f2X2CXc%2F1lWXtV1npkIPSntCVsMJk0%2Fimg.png)

- 변수의 스코프가 선언된 위치에 따라 정해지는 것과 달리 클로저는 선언된 함수 `주변 환경`에 따라 접근할 수 있는 변수가 달라진다. 이러한 환경은 어휘적 환경(Lexical Environment) 라고 한다.


# 2. 역할

### 2-1. 데이터를 보존하는 함수

```javascript
function closure (meow) {
  let parameter;
  return `${parameter}${meow}`;
}

console.log(parameter); // ReferenceError: parameter is not defined (함수 내부에 선언한 변수에 접근 불가)
console.log(meow); // ReferenceError: meow is not defined (매개변수에 접근 불가)
```
- 일반적으로 함수 내부에 선언된 변수는 외부에서 접근할 수 없다. 


```javascript
function closure (meow) {
	let parameter = '안녕하다';

	const greeting = function() {
		return `${parameter}${meow}`;
	}
	return greeting;
}

const getGreeting = closure('냥');
getGreeting();    // '안녕하다냥'
```
- 하지만 클로저를 응용하면 함수 내부 변수는 물론 매개변수에도 접근할 수가 있다.

```javascript
function closure (meow) {
	const greeting = function(parameter) {
		return `${parameter}${meow}`;
	}
	return greeting;
}

const getGreeting = closure('냥');
getGreeting('뭐다');    // '뭐다냥'
```
- closure 안에 선언한 변수를 내부 함수의 파라미터로 넘겨도 잘 적용이 된다.

### 2-2. 커링

- 커링이란, `여러 전달인자를 가진 함수`를 `연속적으로 함수를 리턴`하는 것으로 변경하는 것이다.
- 함수의 일부만 호출하거나, 일부 프로세스가 완료된 상태를 저장하기 용이하다.

```javascript
function sum(a, b) {
  return a + b;
}

function currySum(a) {
	return function(b) {
		return a + b;
	};
}

console.log(sum(10, 20) === currySum(10)(20)) // true
```


### 2-3. 모듈 패턴(캡슐화)

#### cnt 라는 변수의 값을 cntPlus 라는 함수를 이용해서만 변경하고자 할 경우

##### 1.

```javascript
let cnt = 0;
function cntPlus() {
	cnt = cnt + 1;
}
```
- cnt 라는 변수를 선언하고 cntPlus(); 를 호출하면 cnt += 1 되도록 하였다.

```javascript
let cnt = 0;
function cntPlus() {
	cnt = cnt + 1;
}

console.log(cnt);    // 0
cntPlus();
console.log(cnt);    // 1

...

cnt = 100;

console.log(cnt);    // 100
```
- 기능만 따지면 정상적으로 작동이 되겠지만, cnt는 전역변수이기 때문에 cntPlus() 함수뿐만 아니라 다른 곳에서도 수정이 가능하다.

##### 2.

```javascript
function closure() {
	let cnt = 0;
	
	function cntPlus() {
		return cnt = cnt + 1;
	}
	return cntPlus;
}

const cntClosure = closure();
console.log(cntClosure);    // [Function: cntPlus]
cntClosure();               // 1
```
- 이러한 상황을 방지하려면 cnt라는 변수를 지역 변수처럼 만들어서 해당 함수가 아니면 접근하지 못하도록 `캡슐화` 하는 것이다.