# 주요 개발 환경

- Next.js 13.4.19
- TypeScript
- Emotion

# Error message

- Element implicitly has an 'any' type because expression of type 'string' can't be used to index type '{ default: string; danger: string; outline: string; }'.  
- No index signature with a parameter of type 'string' was found on type '{ default: string; danger: string; outline: string; }'.ts(7053)

## 배경

- Next.js + Typescript 를 기반으로 개발 환경을 구성하였고 CSS-in-JS 라이브러리로 Emotion을 사용하고 있습니다.

```typescript
interface StyledProps {
  size: string;
  variant: string;
}

const colors = {
  default: 'rgb(36, 41, 47)',
  danger: 'rgb(207, 34, 46)',
  outline: 'rgb(9, 105, 218)',
};

const sizeStyles = {
  sm: {
    fontSize: '1.2rem',
    padding: '3px 12px',
  },
  md: {
    fontSize: '1.4rem',
    padding: '5px 16px',
  },
  lg: {
    fontSize: '1.6rem',
    padding: '9px 20px',
  },
};
```

- 버튼의 색상과 폰트를 여러가지로 주기 위해 조건을 설정하였고 

```jsx
<S.Button variant="outline" size="md">
완료
</S.Button>
```

- 위와 같이 버튼에 props를 넘겨주어서 조건부 스타일링을 하려고 하였는데요!

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcDYEVp%2FbtstxlhyMc9%2FKeL3npdfBjXDrqBHwkDook%2Fimg.png)

- 각각 선언한 변수를 인덱스 시그니쳐로 참조하려고 할 때 타입 에러가 발생해서 타입을 명시해주려다 학습한 내용을 정리하게 되었습니다.

## 인덱스 시그니쳐(Index Signature)

```javascript
// 문자열로 참조
let foo = {};
foo['hello'] = 'world';

// 객체로 참조
foo[obj] = 'world';

console.log(foo['hello']);   // world
console.log(foo[obj]); // world, toString() 호출
```

- 자바스크립트에서는 객체의 프로퍼티를 참조할 때 문자열와 객체로 액세스가 가능합니다. 이 때 객체로 참조할 때는 런타임시 객체의 toString() 메소드를 호출하게 됩니다.
- 타입스크립트에서는 인덱스 시그니처를 사용할 때 고려해야 할 부분이 조금 더 있습니다.

```typescript
const obj = {
  foo: "hello",
}

let propertyName = "foo"

console.log(obj[propertyName]) // compile error!
```

- 자바스크립트에서는 허용되지만 타입스크립트에서 에러가 발생하는 이유는 **string literal** 타입만 허용되는 곳에 **string** 타입을 사용했기 때문입니다.

```typescript
const a = "Hello World"
let b = "Hello World"
const c: string = "Hello World"
```

- 이 경우 **b**는 let으로 선언이 되었기 때문에 어떤 문자열이든 재할당될 수 있습니다. 따라서 컴파일러는 이것을 string 타입으로 추론합니다.
- **c**는 타입을 명시했기 때문에 string 타입이 됩니다.
- 그러나 **a**는 string 타입이 아니라 **더 좁은 타입**으로 추론합니다. 이것을 **Literal Narrowing** 이라고 하는데요. 무한대의 경우의 수를 가진 string이 아니라 더 구체적인 타입인 **string literal type**으로 생각하는 것입니다.

## 타입스크립트에서 인덱스 시그니쳐 사용하기

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb18tyN%2Fbtsts0ZEytt%2Fb0hP0JWRUcn3sDgCtKyu91%2Fimg.png)

- 우선 인덱스 시그니쳐의 key 타입은 **string, number, symbol, 그리고 template literal** 가 가능합니다.

```typescript
const colors: {[key: string]: string} = {
  default: 'rgb(36, 41, 47)',
  danger: 'rgb(207, 34, 46)',
  outline: 'rgb(9, 105, 218)',
};
```

- 인덱스 시그니처의 타입을 지정할 때는 **{\[key : T\] : U}** 형식으로 사용합니다.
- 대괄호 안에 key의 타입, value의 타입을 적어주면 되는 것이죠!

```typescript
const sizeStyles: { [key: string]: { [subKey: string]: string } } = {
  sm: {
    fontSize: '1.2rem',
    padding: '3px 12px',
  },
  md: {
    fontSize: '1.4rem',
    padding: '5px 16px',
  },
  lg: {
    fontSize: '1.6rem',
    padding: '9px 20px',
  },
};
```

- 위와 같은 중첩된 구조도 마찬가지겠죠?!

---
### 참고 자료

- [인덱스 시그니처(Index Signature) 사용 방법](https://developer-talk.tistory.com/297)
- [TypeScript Deep Dive](https://radlohead.gitbook.io/typescript-deep-dive/type-system/index-signatures)
- [TypeScript에서 string key로 객체에 접근하기](https://soopdop.github.io/2020/12/01/index-signatures-in-typescript/)