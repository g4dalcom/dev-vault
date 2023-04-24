![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbAIkLj%2FbtscArEpPZ1%2F9xBblAr8hlWWsrVDa8lKx1%2Fimg.png)

# Promise 등장 배경

- 자바스크립트에서 비동기 처리를 위해서 전통적으로 콜백 함수를 사용하였다. 
- 함수 내부에서 함수 호출을 통해 비동기 작업의 결과를 받아서 인자로 주면 이를 이용해 후속 처리 작업을 수행하는 것이다.
- 콜백 함수는 중첩될 수록 가독성이 떨어지고 코드의 흐름을 파악하기 어려워진다는 문제가 있는데 특히 여러 개의 비동기 작업을 수행해야 할 때는 이른바 **콜백 지옥(callback hell)** 현상이 발생하게 된다.

```javascript
function increaseAndPrint(n, callback) {
  setTimeout(() => {
    const increased = n + 1;
    console.log(increased);
    if (callback) {
      callback(increased); // 콜백함수 호출
    }
  }, 1000);
}

increaseAndPrint(0, n => {
  increaseAndPrint(n, n => {
    increaseAndPrint(n, n => {
      increaseAndPrint(n, n => {
        increaseAndPrint(n, n => {
          console.log('끝!');
        });
      });
    });
  });
});
```

![](https://blog.kakaocdn.net/dn/dqXplT/btsbSgSHnhX/wqUoq9WTpygNxc3fEWWUDk/img.gif)

## 프로미스로 개선된 비동기 처리

- 자바스크립트의 Promise 객체를 이용하면 콜백 함수에서 들여쓰는 부분을 **.then()** 으로 표현해 체이닝 할 수 있기 때문에 훨씬 가독성있게 코드를 작성할 수 있다.

```javascript
function increaseAndPrint(n) {
  return new Promise((resolve, reject)=>{
    setTimeout(() => {
      const increased = n + 1;
      console.log(increased);
      resolve(increased);
    }, 1000)
  })
}

increaseAndPrint(0)
  .then((n) => increaseAndPrint(n))
  .then((n) => increaseAndPrint(n))
  .then((n) => increaseAndPrint(n))
  .then((n) => increaseAndPrint(n));
```

# Promise 객체

- 프로미스는 비동기 작업의 완료 또는 실패를 나타내는 객체이다. 비동기 작업이 끝날 때까지 결과를 기다리는 것이 아니라 결과를 제공하겠다는 '약속'을 반환한다는 의미에서 Primise라 명명되었다고 한다!

## 객체 생성

- **new** 키워드와 함께 Promise 생성자 함수를 사용한다.
- 생성자 안에 두 개의 매개변수를 가진 콜백 함수를 넣게 되는데 
- 첫 번째 인수는 작업이 성공했을 때(resolve), 두 번째 인수는 작업이 실패했을 때(reject)를 알려주는 객체이다.

```javascript
const myPromise = new Promise((resolve, reject) => {
	// 비동기 작업 수행
    const data = fetch('서버로부터 요청할 URL');
    
    if(data)
    	resolve(data);
    else
    	reject("Error");
})
```

## 객체 처리

- Promise 객체는 비동기 작업이 완료된 이후 후속 작업을 연결시켜 처리할 수 있다.
- 작업이 성공하여 **resolve()** 를 호출하게 되면 **.then**으로 이어지고 resolve() 함수의 매개변수의 값이 .then 메소드의 콜백 함수 인자로 들어가서 .then 메소드 내부에서 그 값을 다룰 수 있게 된다.
- 반대로 처리가 실패하면 reject() 가 호출되어 .catch()로 이어지게 된다.

```javascript
myPromise
    .then((value) => { // 성공적으로 수행했을 때 실행될 코드
    	console.log("Data: ", value);
    })
    .catch((error) => { // 실패했을 때 실행될 코드
     	console.error(error);
    })
    .finally(() => { // 성공하든 실패하든 무조건 실행될 코드
    	
    })
```

## Promise 함수 등록

- 위와 같이 프로미스 객체를 변수에 바로 할당하는 방식을 사용할 수도 있지만 보통은 별도로 함수로 감싸서 사용하는 것이 일반적이라고 한다.

```javascript
// 프로미스 객체를 반환하는 함수 생성
function myPromise() {
  return new Promise((resolve, reject) => {
    if (/* 성공 조건 */) {
      resolve(/* 결과 값 */);
    } else {
      reject(/* 에러 값 */);
    }
  });
}

// 프로미스 객체를 반환하는 함수 사용
myPromise()
    .then((result) => {
      // 성공 시 실행할 콜백 함수
    })
    .catch((error) => {
      // 실패 시 실행할 콜백 함수
    });
```

- 프로미스 객체를 함수로 만들면 다음과 같은 장점이 있다.
	- 재사용성 : 함수로 만들어서 필요할 때마다 호출함으로써 반복되는 비동기 작업에 재사용
	- 가독성 : 비동기 작업의 정의와 사용을 분리함으로써 코드의 구조를 명확하게 파악 가능
	- 확장성 : 여러 개의 프로미스 객체를 반환하는 함수들을 연결하여 복잡한 비동기 로직을 구현 가능


## Promise의 상태

#### Pending(대기) : 처리 진행중

- 프로미스 객체를 생성하고 resolve()나 reject() 메소드를 실행하기 전 대기 상태를 의미한다.

#### Fulfilled(이행) : 처리 완료

- resolve() 메소드가 실행되면서 비동기 처리 로직이 성공적으로 완료된 상태
- 이행 상태가 된 프로미스 객체는 바로 체이닝 된 **.then()** 메소드를 호출하여 처리 결과 값을 받을 수 있게 된다.

```javascript
promise
    .then((data) => {
        console.log("처리 성공")
    })
```

#### Rejected(거부) : 처리 실패

- reject() 메소드가 실행되면서 비동기 처리 로직이 실패한 상태
- 실패 상태가 된 프로미스 객체는 바로 체이닝 된 **.catch()** 메소드를 호출한다.

```javascript
promise
	.catch((error) => {
    	console.log(error);
        console.log("처리 실패 ㅠㅠ")
    })
```


## 프로미스 체이닝

- 프로미스 체이닝이란, 프로미스 핸들러를 연달아 연결해서 여러 개의 비동기 작업을 순차적으로 수행하는 것이다.
- 체이닝이 가능한 이유는 **.then** 핸들러에서 반환하는 값 또한 프로미스 객체이기 때문이다.

```javascript
function plus() {
  return new Promise((resolve, reject) => {
      resolve(100)
  });
}

plus()
    .then((value1) => {
        const data1 = value1 + 50;
        return data1
    })
    .then((value2) => {
        const data2 = value2 + 50;
        return data2
    })
    .then((value3) => {
        const data3 = value3 + 50;
        return data3
    })
    .then((value4) => {
        console.log(value4); // 250 출력
    })
```


## 프로미스 정적 메소드

- 프로미스 객체를 생성 & 초기화하지 않고 바로 사용할 수 있는 메소드가 있다.

### Promise.resolve(), Promise.reject()

- 원래 resolve(), reject() 메소드는 프로미스 객체를 생성하면서 그 안의 콜백 함수의 매개변수를 통해 호출하여 사용한다.

```javascript
// 프로미스 생성자를 사용하여 프로미스 객체를 반환하는 함수
function getPromiseNumber() {
  return new Promise((resolve, reject) => {
      const num = Math.floor(Math.random() * 10); // 0 ~ 9 사이의 정수
      resolve(num); // 프로미스 이행
  });
}
```

- 이러한 과정을 객체 생성 없이 호출하여 사용할 수 있도록 하는 메소드이다.
- 프로미스 객체와 전혀 연관이 없는 함수에서 필요에 따라 프로미스 객체를 반환해 핸들러로 이용할 수 있다.

```javascript
// 프로미스 객체와 전혀 연관없는 함수
function getRandomNumber() {
  const num = Math.floor(Math.random() * 10); // 0 ~ 9 사이의 정수
  return num;
}

// Promise.resolve() 를 사용하여 프로미스 객체를 반환하는 함수
function getPromiseNumber() {
  const num = getRandomNumber(); // 일반 값
  return Promise.resolve(num); // 프로미스 객체
}

// 핸들러를 이용하여 프로미스 객체의 값을 처리하는 함수
getPromiseNumber()
    .then((value) => {
      console.log(`랜덤 숫자: ${value}`);
    })
    .catch((error) => {
      console.error(error);
    });
```

### Promise.all()

- 배열, Map, Set에 포함된 여러 개의 프로미스 요소들을 한꺼번에 비동기 처리할 때 사용된다.
- 모든 비동기 처리가 이행(fulfilled)될 때까지 기다렸다가 한꺼번에 모아서 **.then** 핸들러를 실행하는 형태로 볼 수 있다.
- 여러 개의 API요청을 보내고 모든 응답을 받아서 한 번에 처리해야 하는 경우에 사용될 수 있다.

```javascript
// 1. 서버 요청 API 프로미스 객체 생성 (fetch)
const api_1 = fetch("요청 URL 1");
const api_2 = fetch("요청 URL 2");
const api_3 = fetch("요청 URL 3");

// 2. 프로미스 객체들을 묶어 배열로 구성
const promises = [api_1, api_2, api_3];

// 3. Promise.all() 메서드 인자로 프로미스 배열을 넣어, 모든 프로미스가 이행될 때까지 기다리고, 결과값을 출력
Promise.all(promises)
    .then((results) => {
      // results는 이행된 프로미스들의 값들을 담은 배열.
      // results의 순서는 promises의 순서와 일치.
      console.log(results); // [users1, users2, users3]
    })
    .catch((error) => {
      // 어느 하나라도 프로미스가 거부되면 오류를 출력
      console.error(error);
    });
```


### Promise.allSettled()

- Promise.all() 과 역할은 비슷하지만 Promise.all()이 프로미스 중 하나라도 reject 되면 오류를 출력하는 것과 달리, 모든 프로미스 각각의 상태와 값을 모아놓은 배열을 반환한다.

```javascript
// 1초 후에 1을 반환하는 프로미스
const p1 = new Promise(resolve => setTimeout(() => resolve(1), 1000));

// 2초 후에 에러를 발생시키는 프로미스
const p2 = new Promise((resolve, reject) => setTimeout(() => reject(new Error('error')), 2000));

// 3초 후에 3을 반환하는 프로미스
const p3 = new Promise(resolve => setTimeout(() => resolve(3), 3000));

// 세 개의 프로미스의 상태와 값 또는 사유를 출력
Promise.allSettled([p1, p2, p3])
	.then(result => console.log(result));
```

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbZbGKn%2FbtsbUP0ItLP%2FXMPpAFQLuk9cykkdCUGuvK%2Fimg.png)


### Promise.any(), Promise.race()

- Promise.all()이 모든 프로미스가 완료되어야만 결과를 반환한다면, **Promise.any()**는 주어진 프로미스 중 하나라도 완료되면 가장 먼저 완료된 프로미스 하나만 반환한다.
- 그리고 Promise.any()는 가장 먼저 이행(fulfilled) 상태가 된 프로미스를 반환한다면 **Promise.race()** 는 이행, 실패 여부 상관없이 가장 먼저 **처리가 끝난** 프로미스 결과값을 반환한다.

---

### 참고 자료

- [드림코딩 프로미스 개념부터 활용까지](https://www.youtube.com/watch?v=JB_yU6Oe2eE)
- [자바스크립트 Promise 쉽게 이해하기](https://joshua1988.github.io/web-development/javascript/promise-for-beginners/)
- [자바스크립트 Promise 개념 & 문법 정복하기](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EB%B9%84%EB%8F%99%EA%B8%B0%EC%B2%98%EB%A6%AC-Promise#1._pending_%EC%83%81%ED%83%9C)