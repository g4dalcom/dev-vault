# 타입스크립트의 Interface

자바의 인터페이스가 추상 메소드를 가지면서 설계와 구현을 분리하기 위한 기능이라면, 타입스크립트의 인터페이스는 **객체를 위주로 다루면서 그 객체의 타입을 정의해놓은 설계도**라고 할 수 있다.

```typescript
interface Fruit {
	name: string;
	price: number;
}
```

interface명은 첫 글자를 대문자로 한다.

```typescript
// 객체의 타입을 인터페이스로 지정
const fruit: Fruit = {
	name: mango;
	price: 1000;
}

// 매개변수의 타입과 반환타입 또한 인터페이스로 지정할 수 있다.
function selling(item: Fruit): Fruit {
	// ...
}
```


## 인터페이스 & 타입 별칭

인터페이스와 type alias는 객체 구조 타입을 선언할 때 사용할 수 있다는 점에서 그 기능이 같아보인다.

```typescript
// 인터페이스
interface Fruit {
	name: string;
	price: number;
	sell: () => void;
}

// 타입 별칭
type Fruit = {
	name: string;
	price: number;
	sell: () => void;
}

// 구현
const fruit: Fruit = {
	name: "mango",
	price: 1000,
	sell() {},
}
```


### 타입 별칭의 특징

타입 별칭은 새로운 타입값을 생성하는 것이 아니라 정의한 타입에 대해 나중에 쉽게 참고할 수 있게 이름을 부여하는 데 특화되어 있다.

VSCode에서 타입에 커서를 올렸을 때 인터페이스와 타입별칭의 차이를 보면,

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FEXJkN%2Fbtsh4PG7pLD%2FYmfbRHUsSkiZufY5LHEU40%2Fimg.png)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbgc0tx%2Fbtsh4kglu0D%2FNkP6alGkM0P7aPjlwleppk%2Fimg.png)

타입 별칭의 경우는 참조하고 있는 타입의 프로퍼티를 확인할 수 있다.


### 타입의 확장

```typescript
interface Fruit {
	name: string;
}

interface MyFruit extends Fruit {
	price: number;
}

const fruit: MyFruit = {
	name: "mango",
	price: 1000,
}
```

```typescript
type Fruit = {
	name: string;
}

type MyFruit = Fruit & {
	price: number;
}

const fruit: MyFruit = {
	name: "mango",
	price: 1000,
}
```

인터페이스는 **extends** 키워드, 타입 별칭은 **인터렉션 타입(&)** 을 이용해 확장이 가능하다.

인터페이스와 클래스가 같은 extends 키워드를 사용하는데 클래스는 반드시 하나만 extends 할 수 있는 반면에 인터페이스는 여러 개를 extends 할 수 있다.

```typescript
interface Fruit {
	name: string;
	price: number;
}

interface Vegetable {
	isVegatable: boolean;
}

interface MyFruit extends Fruit, Vegetable {
	sell: () => void;
}

const store: MyFruit = {
	name: "mango",
	price: 1000,
	isVegatable: false,
	sell() {};
}
```


## 선택적 프로퍼티

인터페이스로 정의한 프로퍼티들은 해당 인터페이스를 타입으로 가질 때 모두 구현을 해야되는 것이 기본이다.

```typescript
interface Fruit {
	name: string;
	price: number;
	sell: () => void;
}

const myFruit: Fruit = {
	name: "mango";
}  // price와 sell을 구현하지 않아서 Error!!
```

위와 같이 name 프로퍼티만 필요하다면 name 만을 갖는 인터페이스를 새로 정의해야 할까?

이러한 불편함은 선택적 프로퍼티라는 연산자를 이용해서 해결할 수 있다.

```typescript
interface Fruit {
	name: string;
	price?: number;
	sell?: () => void;
}

const myFruit: Fruit = {
	name: "mango",
}
```

인터페이스 프로퍼티의 **콜론 앞에 물음표(?)** 를 붙이면 해당 프로퍼티는 Optional한 속성이 된다. 따라서 구현해도 되고 하지 않아도 되는 것이다.

```typescript
interface Fruit {
	name: string;
	price: number | undefined;
}
```

선택적 프로퍼티는 위와 같이 Union 타입과 같은 의미이기도 하다.
선택 속성을 사용하는 로직이 있다면 컴파일러가 선택 속성의 유무를 확신할 수 없기 때문에 선택 속성을 사용할 때는 유니온 타입에서의 타입 가드 기법을 써주어야 한다.

```typescript
const fruit: Fruit = {
	name: "mango";
}

if (fruit.price < 2000) {};
// Error!

// 타입 가드 기법
if (fruit.price && fruit.price < 2000) {};
```


## 읽기 전용 프로퍼티(readonly)

객체를 처음 생성할 때 값을 할당하고 그 이후에는 변경할 수 없는 속성을 의미한다.
변수에서 사용하는 const와 유사하다.

속성 앞에 **readonly** 를 붙여서 적용한다.

```typescript
interface Fruit {
	name: string;
	price: number;
	readonly isVegatable: boolean;
}

const fruit: Fruit = {
	name: "mango",
	price: 1000,
	isVegatable: false,
}

fruit.isVegatable = true;  // Error!
```


## 인터페이스 호환

기존에 만들어진 인터페이스에 프로퍼티를 추가하고 싶을 때 새롭게 인터페이스를 정의하는 것이 아니라 같은 이름의 인터페이스에 새로운 프로퍼티를 정의하면 기존 인터페이스와 합쳐지게 된다.

```typescript
interface Fruit {
	name: string;
}

interface Fruit {
	price: number;
}

const fruit: Fruit = {
	name: "mango",
	price: 1000,
}
```


## 인터페이스 함수 타입

인터페이스가 객체의 타입을 정의해놓은 설계도라고 하였는데 이를 함수에 적용하면, 함수의 타입을 정의해놓는 기능이라고 볼 수 있다.

```typescript
interface login {
	(username: string, password: string): boolean;
}

let loginUser: login = function(id, pw) {
	console.log('로그인 성공!');
	return true;
}
```

객체의 타입을 정의할 때 **타입명: type**을 썼다면 함수에서는 함수의 모양(인자, 리턴) 타입을 쓴다.

이것은 **호출 시그니처(Call Signature)** 라고도 한다.

또한 함수도 프로퍼티를 가질 수가 있다.

```typescript
interface GetText {
	(name: string, age: number): string;
	total?: number;  // 함수의 프로퍼티
}

const getText: GetText = function(name, age) {
	if (getText.total !== undefined) {
		getText.total += 1;
	}
	return '';
} 
```


## 인터페이스 클래스 타입

인터페이스로 클래스를 정의할 때는 **implements** 키워드를 이용한다. 이 때 클래스는 반드시 인터페이스에 정의된 프로퍼티들을 구현해야 한다.

```typescript
interface IUser {
	name: string;
	getName(): string;
}

class User implements IUser {
	name: string;

	constructor(name: string) {
		this.name = name;
	}

	getName() {
		return this.name;	
	}
}
```


---

### 참고 자료

[타입스크립트 인터페이스 활용 정리](https://inpa.tistory.com/entry/TS-%F0%9F%93%98-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4-%F0%9F%92%AF-%ED%99%9C%EC%9A%A9%ED%95%98%EA%B8%B0)
[타입스크립트 핸드북 - 인터페이스](https://joshua1988.github.io/ts/guide/interfaces.html)
[인터페이스 vs 타입별칭](https://yceffort.kr/2021/03/typescript-interface-vs-type)