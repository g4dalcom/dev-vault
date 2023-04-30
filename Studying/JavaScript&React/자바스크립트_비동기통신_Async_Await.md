# 등장배경

- 콜백 함수가 중첩될 수록 코드가 복잡해진다는 **콜백 지옥**을 해결하기 위해 Promise가 등장하였는데
- Promise 또한 체인이 반복되면 **프로미스 지옥**이 펼쳐진다.

### Promise Hell 예시

```javascript
fetch("URL")
  .then((response) => {
    if (response.ok) {
      return response.json();
    } else {
      throw new Error("Error");
    }
  })
  .then((users) => {
    return users.map((user) => user.login);
  })
  .then((logins) => {
    return logins.join(", ");
  })
  .then((result) => {
    console.log(result);
  })
  .catch((error) => {
    console.error(error);
  });
```

### 개선된 async/await 예시

```javascript
try {
    const response = await fetch("URL");
    
    if (response.ok) {
      const users = await response.json();
      const logins = users.map((user) => user.login);
      const result = logins.join(", ");
      console.log(result);
    } else {
      throw new Error("Error");
    }
    
} catch (error) {
	console.error(error);
}
```
- then을 남발하여 코드의 흐름을 한 번에 파악하기 어려웠던 프로미스보다 
- 마치 함수의 리턴값을 변수가 받는 정의문 형식으로 되어 있어서 코드의 의도를 파악하기 훨씬 수월하다!

### fetch 예시

```javascript
fetch(url)
    .then(res => res.json())
    .then(data => {
      console.log(data);
    })
```
- 기존 .then을 이용한 fetch

```javascript
async function func() {
    const res = await fetch(url);
    const data = await res.json();
    console.log(data);
}
func();
```
- async/await을 이용한 fetch

# async/await 문법

- **function 키워드 앞에 async**를 붙여주고
- **비동기 처리가 되는 부분 앞에 await**을 붙여주면 된다.

```javascript
// 함수 선언식
async function func1() {};

// 함수 표현식
const func2 = async () => {};
```

## Promise 와 비교

```javascript
function time(ms) {
  return new Promise(resolve => {
    setTimeout(() => {
      console.log(ms);
      resolve()
    }, ms);
  });
}
```

### Promise 방식

```javascript
function promise() {
  time(1000)
      .then(() => {
        return time(2000);
      })
      .then(() => {
        return Promise.resolve('종료');
      })
      .then(result => {
        console.log(result);
      });
}

promise()
```

### async/await 방식

```javascript
async function asyncAwait() {
  await time(1000);
  await time(2000);
  const result = await Promise.resolve('종료');
  console.log(result);
}

asyncAwait();
```

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcy6BCh%2FbtsddZ2My4v%2FJwhHkMQV0gIiZePH4OkkDk%2Fimg.png)
- 결과를 보면 async/await이 Promise 객체를 반환하는 것을 알 수 있다.

> async/await은 문법적으로 개선이 된 것일뿐 내부적으로는 Promise를 사용해서 비동기 처리를 하는 것!

- 따라서 async/await은 Promise.all() 과 함께 자주 사용되기도 한다.

```javascript
// 1초 뒤에 "apple" 을 반환
function getApple(){
  return new Promise( (resolve, reject) => {
    setTimeout(() => resolve("apple"), 1000);
  })
}

// 1초 뒤에 "banana" 를 반환
function getBanana(){
  return new Promise( (resolve, reject) => {
    setTimeout(() => resolve("banana"), 1000);
  })
}

async function getFruites(){

  let [ a, b ] = await Promise.all([getApple(), getBanana()]); 
  console.log(`${a} and ${b}`);
}

getFruites();

// apple and banana
```


## 에러 처리

### Promise 방식

```javascript
function fetchItem(url) {
  fetch(url)
    .then(res => res.json())
    .then(data => {
      console.log(data);
    })
    .catch(err => {
      console.error(err);
    });
}
```
- .then 과 같은 레벨에 .catch를 명시함으로써 에러를 처리한다.

```javascript
async function fetchItem() {
	try {
		const res = await fetch(url);
		const data = await res.json();
		console.log(data);
	} catch(err) {
		console.error(err);
	}
}
```
- 일반적인 동기 코드를 에러 처리하듯이 try/catch문으로 처리할 수가 있어서 가독성이 훨씬 좋다!


---
### 참고 자료

- [드림코딩 유튜브](https://youtu.be/aoQSOZfz3vQ)
- [10분 테코톡 프론트엔드에서의 비동기](https://youtu.be/fsmekO1fQcw)
- [모던 자바스크립트 튜토리얼](https://ko.javascript.info/async-await)
- [자바스크립트 Async/Await 개념 & 문법 정복](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EB%B9%84%EB%8F%99%EA%B8%B0%EC%B2%98%EB%A6%AC-async-await)