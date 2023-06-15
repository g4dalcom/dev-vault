# CSS 개발 방식

## CSS-in-CSS

- 렌더링할 때 모든 CSS 요소를 로딩하기 때문에 **인터렉티브한 웹 페이지**를 구성할 때 **사용자 편의**를 반영하기 좋다.

## CSS-in-JS

- **해당 컴포넌트가 렌더링 될 때만 스타일 태그를 생성**하기 때문에 **컴포넌트 기반 프로젝트에 유용**하다.
- 각 컴포넌트의 라이프 스타일에 맞게 스타일을 적용할 수 있고 자바스크립트 코드를 활용할 수 있기 때문에 **동적 스타일 적용**이 자유롭다.
- 자바스크립트 해석 과정이 필요하기 때문에 CSS 모듈 방식에 비해 성능이 느리다.


# CSS 라이브러리

## CSS-in-CSS 종류

### CSS-module

- CSS를 모듈화하여 사용하는 방법
- 기본적으로 CSS 클래스의 범위가 정해져있다는 점에 대한 이점은 CSS-in-JS와 동일하지만 순수 CSS 파일을 유지하기 때문에 CSS-in-JS가 가지고 있는 자바스크립트 해석 과정이 필요없다.
- **state of css** 에서는 CSS-in-JS로 분류하기도 하는데 그 와중에 인지도, 사용률, 만족도 모두 상위권이다.

```javascript
{이름}.module.css
```

```css
/* Component1.module.css */

.bg {
	background: green;
}
```

```javascript
import React from 'react'
import style from './Component1.module.css'

export default function Component1() {
        return <div className={style.bg}>
		        div Tag</div>
}
```

- CSS 클래스를 만들면 자동으로 고유한 className을 만들어서 scope를 지역적으로 제한한다.
- 컴포넌트 단위로 스타일을 적용할 때 유용하다!

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FM8nAs%2Fbtsj3lJ7EtG%2FFKL5hINxMAh5hNicC99Zg1%2Fimg.png)

#### 장점

- Scope가 지역적으로 제한되기 때문에 전역 오염을 방지
- 고유한 클래스로 인해 클래스명 작명에 대한 부담이 없음

#### 단점

- 컴포넌트별로 CSS파일을 관리하기 때문에 파일이 많아짐
- 동적으로 클래스를 구성하려면 별도의 모듈 설치가 필요


### CSS 전처리기(Preprocessor)

- CSS를 프로그램처럼 관리하려는 방식이다.
- 특별한 구문(Syntax)을 가지고 프로그래밍 개념을 사용하여 컴파일을 통해 CSS를 생성한다.

#### 주요 모듈

- Sass, Less, Stylus, PostCSS, Assemble CSS
- [CSS전처리기란](https://velog.io/@eunoia/CSS-%EC%A0%84%EC%B2%98%EB%A6%AC%EA%B8%B0%EB%9E%80)
- [CSS전처리기 종류별 특징](https://kimby.tistory.com/54)


#### 장점

- 공통 요소를 변수나 함수로 대체할 수 있는 재사용성(테마 공유 등)
- 중첩, 상속 등으로 코드 관리가 용이함

#### 단점

- 별도의 전처리기를 위한 도구가 필요
- 컴파일 하는 데 시간 소요



## CSS-in-JS 종류

### Styled-component & Emotion

- CSS-in-JS 에서 인지도가 있는 두 라이브러리를 비교해보았다.
- 두 라이브러리는 모두 CSS 혹은 객체 표기법을 통해 스타일링을 한다.

```javascript
/* styled component */

// 탬플릿 리터럴 방식
const Title = styled.h1`
  font-size: 1.5em;
  text-align: center;
  color: palevioletred;
`
render(<Title>Hiya!</Title>)

// 객체 표기법
const button = styled.button({
  fontSize: '1.5em',
  textAlign: 'center',
  color: 'palevioletred'
});
```

```javascript
/* emotion */

// 탬플릿 리터럴 방식
const Button = styled.button`
  padding: 32px;
  background-color: hotpink;
  font-size: 24px;
  border-radius: 4px;
  color: black;
  font-weight: bold;
  &:hover {
    color: white;
  }
`

render(<Button>Hey! It works.</Button>)

// 객체 전달 방식
render(
  <h1
    className={css`
      font-size: 1.5em;
      text-align: center;
      color: palevioletred;
    `}
  >
    Hiya!
  </h1>
)

// 객체 표기법
const titleStyles = css({
  fontSize: '1.5em',
  textAlign: 'center',
  color: 'palevioletred'
})

render(<h1 className={titleStyles}>Hiya!</h1>)

```

- 보다시피 전반적인 스타일 기능과 문법은 매우 유사하다.
- 탬플릿 리터럴 방식을 사용했을 경우 태그의 구조를 한 눈에 알아보기 어렵다는 단점도 있지만 네이밍 컨벤션을 사용하여 개선하는 방법도 있다!
- [\[React\] Styled-components Naming](https://heycoding.tistory.com/m/71)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdUX8em%2FbtsjZ9YoCXI%2FEENKdvatr3M6meHvPELfqk%2Fimg.png)

- Style-component가 좀 더 인지도가 높아보이지만 emotion의 경우 v10에서 v11이 되면서 @emotion/core 에서 @emotion/react로 패키지 이름이 바뀌었다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcUaVia%2Fbtsj5QKX4FI%2FyMMcptT65MKegmKWNqDi30%2Fimg.png)
- 아직 v10을 사용하는 개발자도 있기 때문에 v10, v11 패키지를 모두 합하면 style-component보다 더 많은 개발자들이 사용한다고 보는 시각도 있다.
- Emotion의 경우 Styled-component에 비해 번들 크기가 좀 더 작고 속도가 좀 더 빠르다는 장점이 있으나 유의미한 차이는 아니라고 한다.


### Styled-JSX

- Next.js 에 기본적으로 내장되어 있는 CSS-in-JS 기반 모듈
- CSS를 캡슐화하여 다른 구성 요소에 영향을 미치지 않도록 하는 특징을 가지고 있다.

#### 기본 사용법

##### 탬플릿 리터럴

```javascript
<>
  <div className="msg">Hello, World!</div>
  <style jsx>`
    .msg {
      font-size: 20px;
    }
  `</style>
</>
```
- style 태그에 jsx 속성을 준 다음 **\`{ }\`** 에 CSS를 넣어준다.
- 기본적으로 styled-jsx는 사용된 컴포넌트에만 영향을 미치며 전역으로 활용하려면 global 속성을 사용한다.

##### 전역 속성

```javascript
<div>
  <div className="msg">Hello, World!</div>
  <style jsx global>`
    .msg {
      font-size: 20px;
    }
  `</style>
</div>
```

##### 스타일 객체 활용

```javascript
import css from "styled-jsx/css";

// 스타일 객체
const style = css`
  .title {
    color: red;
  }
`;

// 컴포넌트 본체
const index = () => {
  return (
    <>
      <div className="title">Hello, World!</div>
      <style jsx>{style}</style>
    </>
  );
};
```
- 비즈니스 로직과 스타일을 분리할 수 있게 되지만 동적인 스타일을 부여할 수 없다.

##### 정적인 스타일과 동적인 스타일(조건부) 분리

```javascript
import css from "styled-jsx/css";
import { useState } from "react";

// 정적 스타일 (상태에 영향을 받지 않는)
const style = css`
  button {
    background-color: #ff23ff;
  }
`;

const index = () => {
  const [isRed, setIsRed] = useState(false);
  return (
    <>
      <div className="title">Hello, World!</div>
      <button
        onClick={() => {
          setIsRed((prev) => !prev);
        }}
      >
        색상 변경
      </button>
      <style jsx>{style}</style>
      // 동적인 스타일 (상태에 따라 색상 변경)
      <style jsx>
        {`
          .title {
            color: ${isRed ? "red" : "black"};
          }
        `}
      </style>
    </>
  );
};

export default index;
```

- 기존에 많이 사용하던 CSS-in-JS의 사용방식과 많이 다르고 레퍼런스가 적어서 적용을 할 때 러닝커브가 꽤나 발생할 것 같다.


## Utility First CSS

- 맞춤형 디자인을 빠르게 구축하기 위한 유틸성 CSS 라이브러리

### Tailwind CSS

#### 기본 사용법

- className에 CSS 속성을 적어주는 방식

```javascript
/* 가운데 정렬 */
<div className="flex justify-center items-center">
  <h1>Awesome Tailwind!!</h1>
</div>
```


#### 장점

- **정해놓은 클래스 이름을 가져와서 스타일링**을 하는 방식이라서 클래스 이름을 고민하지 않아도 됨
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdIYII4%2FbtsjYtbEi78%2F7mIl8FsNuYvaM9UkoPtNw1%2Fimg.png)

- 반응형 스타일도 클래스만 가져오면 되기 때문에 적용하기 용이함 + 다크모드
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FOIM0V%2Fbtsj3oNDT95%2FmaKtOSYPuQW5wKKf8ykFw1%2Fimg.png)

- 화면과 스타일링을 한 번에 개발할 수 있다는 장점

#### 단점

- 적응 기간이 필요하다(러닝 커브)
- 코드 가독성이 떨어진다.
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FAfpE2%2FbtsjYrETrre%2FEBU9vSJbKvNbTfXtwFTuF1%2Fimg.png)

- 물론 위 단점을 개선하는 @apply와 같은 기능도 존재한다!


# 마무리

- 테마와 같은 공통 영역이나 일부 유틸리티는 CSS-in-JS만으로 쉽게 해결이 어렵기 때문에 대부분 기존 CSS방식이나 CSS module과 같은 기술을 병행한다고 한다.
- next.js는 9.2 버전부터 CSS module을 기본으로 지원한다.
- 소규모 프로젝트로써 개발자가 마크업과 JS를 모두 작성하는 경우, 컴포넌트 단위 개발인 경우, 빠르게 작성하고 테스트하는 것이 중요한 경우에는 CSS-in-JS가 유리하고
- 어느 정도 규모를 갖춰서 성능도 고려해야 하는 경우 CSS module이나 +tailwind 와 같은 유틸리티 구성이 보통은 좋다고 한다!

---
### 참고 자료

- [CSS in JS는 무조건 더 좋을까?](https://jthcast.dev/posts/is-css-in-js-the-best/)
- [css-in-js와 in-css 중에서 어떤 방식이 더 적합할까?](https://velog.io/@willy4202/styled-components%EC%9D%98-%EC%82%AC%EC%9A%A9%EC%9D%B4%EC%9C%A0%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90.-css-in-js)
- [웹 컴포넌트 스타일링 관리 : CSS-in-JS vs CSS-in-CSS](https://www.samsungsds.com/kr/insights/web_component.html)
- [CSS module](https://pxd-fed-blog.web.app/css-module/)
- [카카오웹툰은 CSS를 어떻게 작성하고 있을까?](https://fe-developers.kakaoent.com/2022/220210-css-in-kakaowebtoon/)
- [Tailwind CSS 사용법, 장점과 단점](https://onlydev.tistory.com/127)
- [TailwindCSS 사용하기](https://leekihyun.tistory.com/entry/TailwindCSS-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)
- [\[React\] emotion과 styled-component의 차이](https://80000coding.oopy.io/7ad296c7-8832-4951-9cf7-074a196d42ea)
- [Next.js with Styled-JSX 이해하기](https://velog.io/@ka0son/NextJs)
- [넥스트를 꾸며줘 : styled-jsx 활용하기](https://merrily-code.tistory.com/56)