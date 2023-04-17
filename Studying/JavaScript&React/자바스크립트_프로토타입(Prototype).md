# 개요

```javascript
function Car() {
	this.tire = 4;
	this.handle = 1;
}

let kona = new Product();
let spark = new Product();

console.log(kona.tire); // 4
console.log(spark.tire); // 4
```
- kona와 spark 인스턴스를 생성하게 되면 Car가 가지고 있는 tire와 handle이라는 변수가 각각 메모리에 할당이 된다.
- 객체를 만들 때마다 새로운 메모리 공간을 사용하게 되는데 이러한 문제를 프로토타입으로 해결할 수 있다.

```javascript
function Car() {
}

Car.prototype.tire = 4;
Car.prototype.handle = 1;

let kona = new Product();
let spark = new Product();

console.log(kona.handle); // 1
console.log(spark.handle); // 1
```
- Car.prototype라는 공통으로 사용하는 Object를 만들어서 Car가 만들어내는 인스턴스들이 상속받아서 사용할 수 있도록 하는 것이다.


# Prototype Object

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fv6UXP%2Fbtr3TIbuzHe%2FUGc9TYhenqoSYXsAPf63eK%2Fimg.png)

> 함수를 정의하면 함수가 생성됨과 동시에 Prototype Object도 같이 생성이 된다. 


![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc7o2kH%2Fbtr3XXS7T4Q%2FCiYyX50fukExXhzUKOMnY0%2Fimg.png)
- Prototype Object는 기본적인 속성으로 constructor와 \_\_proto\_\_를 가지고 있으며 Product() 함수는 prototype라는 속성을 통해 Prototype Object에 접근할 수 있다.


![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FFcwRH%2Fbtr3SYrXHcQ%2FxseJE1kJ73b2uPq1ZkTHEK%2Fimg.png)
- Prototype Object는 일반적인 객체처럼 속성을 추가할 수 있다. 이 속성은 해당 함수를 통해 생성된 모든 인스턴스들이 참조할 수 있다.

# Prototype Chain

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FHiCSU%2Fbtr34LEDOL5%2FNPtStLEzKw0Ce58MFOid51%2Fimg.png)

> prototype 속성이 함수만 가지고 태어나는 것과 달리 \_\_proto\_\_ 속성은 모든 인스턴스가 가지고 있는 속성으로, \_\_proto\_\_ 는 인스턴스가 생성될 때 조상이었던 함수의 Prototype Object를 가리킨다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdnJ4uk%2Fbtr3TJO7m4Z%2F1fS724tVokXydgPDC6OlWk%2Fimg.png)
- 인스턴스가 해당 속성을 직접 가지고 있지 않으면 그 속성을 찾을 때까지 상위 프로토타입을 탐색한다.
- 최상위인 Object의 Prototype Object에서도 찾지 못하면 그 때 undefined를 리턴한다.
- 이렇게 \_\_proto\_\_속성을 통해 상위 프로토타입과 연결되어 있는 형태를 프로토타입 체인이라고 한다!
- 이러한 프로토타입 체인 구조 때문에 모든 객체는 Object의 자식이며 Object Prototype Object의 속성들을 사용할 수가 있다.