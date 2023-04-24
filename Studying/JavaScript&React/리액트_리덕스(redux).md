# 리덕스란?
- 자바스크립트 상태 관리 라이브러리

### 리덕스가 왜 필요할까?
- 프로젝트 규모가 커질 수록 컴포넌트 개수도 많아질 것이고 그에 따라 서로 다른 컴포넌트끼리 공유하는 상태(state) 또한 많아진다.
- **리액트**는 **부모 컴포넌트에서 자식 컴포넌트로 데이터가 흐르는 단방향 패턴**이며 자식 컴포넌트들 간 다이렉트로 데이터 전달이 불가능하다.

#### 리액트만으로 상태를 관리할 때 문제점

![](https://blog.kakaocdn.net/dn/daSQwb/btsbVcIZjCa/susnkWTUdMyZq28iWCgWWk/img.gif)
###### Props drilling 이슈
- 하위 컴포넌트에서 상위 컴포넌트에 있는 상태를 변화시키기 위해서는 여러 번의 props 전달을 통해야 한다.
- 전달만을 위한 props가 생기기 때문에 코드가 복잡해지고 유지보수가 어려워지며, 불필요한 리렌더링이 많아지는 단점이 있다.

#### 리덕스를 이용할 때 이점

![](https://blog.kakaocdn.net/dn/CNL4d/btsbVdHZJHr/DpTLqnEHaLsxrH3G4KfTkK/img.gif)
- 리덕스를 이용하면 Store라는 전역 상태 저장소에서 상태를 꺼내쓸 수 있기 때문에 불필요한 컴포넌트간 전달이 사라지고 필요한 컴포넌트만 리렌더링 할 수 있다.

## 리덕스의 구조

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fconhau%2FbtscjjteRww%2FS6ekykqhsclJ8KKKUv05pk%2Fimg.png)

### Store
- 상태가 관리되는 단 하나의 저장소로 필요한 상태들과 리듀서가 저장되어 있다.

###### Redux

```javascript
import React from 'react'
import { render } from 'react-dom'
import App from './components/App'

import { createStore } from 'redux'
import { Provider } from 'react-redux'
import counterReducer from './reducers'

const store = createStore(counterReducer)

render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```
- store를 생성해서 reducer를 연결하는 작업
- Provider로 App 컴포넌트를 감싼 것은, App컴포넌트에게 store 접근 권한을 준 것이라고 할 수 있다.


###### Redux-toolkit

```javascript
import { configureStore } from '@reduxjs/toolkit'

export default configureStore({
	reducer: {
		counter: counterReducer
	}
})
```
- 현재 createStore는 deprecated 되어 있으며 공식문서에서는 redux-toolkit의 configureStore의 사용을 권장하고 있다!

### Reducer
- Dispatch에게서 현재의 state와 action을 인자로 받아서 store에 접근하여 전달받은 Action 객체의 상태를 변경시키는 함수이다.
- Reducer는 사이드이펙트가 발생하면 안 되므로 순수함수여야 한다!

###### Redux

```javascript
const count = 1

const counterReducer = (state = count, action) => {

  switch (action.type) {

    case 'INCREASE':
			return state + 1

    case 'DECREASE':
			return state - 1

    default:
      return state;
	}
}
```

###### Redux-toolkit

```javascript
import { createSlice } from '@reduxjs/toolkit'

export const counterSlice = createSlice({
  name: 'counter',
  initialState: {
    value: 0
  },
  reducers: {
    increment: state => {
      state.value += 1
    },
    decrement: state => {
      state.value -= 1
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload
    }
  }
})


export const { increment, decrement, incrementByAmount } = counterSlice.actions

export default counterSlice.reducer
```

### Action
- 상태를 변화시키려는 의도를 표현한 객체로 action type와 전송할 데이터(payload)로 이루어져 있다.

```javascript
// payload가 필요 없는 경우
{ type: 'INCREASE' }

// payload가 필요한 경우
{ type: 'SET_NUMBER', payload: 5 }
```

##### Action creator
- Action이 동작을 표현한 객체라면 Action creator는 Action을 생성해 실제 객체로 만들어주는 함수이다.

```javascript
export const increase = () => {
  return {
    type: 'INCREASE',
  };
};

export const decrease = () => {
  return {
    type: 'DECREASE',
  };
};

// payload가 필요한 경우
export const setNumber = (num) => {
  return {
    type: 'SET_NUMBER',
    payload: num
  }
}
```


### Dispatch
- Reducer로 Action을 전달해주는 함수이다. 따라서 Dispatch의 전달인자로 Action 객체가 들어가게 된다.

#### Redux hooks

###### useDispatch()
- Action 객체를 Reducer로 전달해주는 Dispatch 함수를 반환하는 메소드

```javascript
import { useDispatch } from 'react-redux'

const dispatch = useDispatch();
dispatch(increase());
```

###### useSelector()
- 컴포넌트와 state를 연결해서 Redux의 state에 접근할 수 있게 해주는 메소드

```javascript
import { useSelector } from 'react-redux'

const counter = useSelector(state => state);
```


###### Redux

```javascript
import React from 'react';
import './style.css';

import { useDispatch } from 'react-redux';
import { increase, decrease } from './index.js';

export default function App() {
  const dispatch = useDispatch();
  const state = useSelector((state) => state);

  const plusNum = () => {
    dispatch(increase());
  };

  const minusNum = () => {
    dispatch(decrease());
  };

  return (
    <div className="container">
      <h1>{`Count: \${state}`}</h1>
      <div>
        <button className="plusBtn" onClick={plusNum}>
          +
        </button>
        <button className="minusBtn" onClick={minusNum}>
          -
        </button>
      </div>
    </div>
  );
}
```


###### Redux-toolkit

```javascript
import React from 'react'
import { useSelector, useDispatch } from 'react-redux'
import { decrement, increment } from './counterSlice'
import styles from './Counter.module.css'

export function Counter() {
  const count = useSelector(state => state.counter.value)
  const dispatch = useDispatch()

  return (
    <div>
      <div>
        <button
          aria-label="Increment value"
          onClick={() => dispatch(increment())}
        >
          Increment
        </button>
        <span>{count}</span>
        <button
          aria-label="Decrement value"
          onClick={() => dispatch(decrement())}
        >
          Decrement
        </button>
      </div>
    </div>
  )
}
```


## 리덕스의 흐름

![](https://blog.kakaocdn.net/dn/lxFy6/btsb2TB9EkP/kAmvpCJ7akifkKnZ6Cduu0/img.gif)
- UI에서 컴포넌트 내에 존재하는 이벤트가 호출
- 이벤트와 연결된 **액션 생성자**가 호출
- 액션 생성자에서 생성된 **액션**이 호출됩니다.
- **디스패치**가 **액션**을 **리듀서**로 전달
- **리듀서**에서 디스패치된 액션에 따라 상태값을 변경
- 변경사항이 렌더링

---
### 참고 자료

- [생활코딩 react-redux(2022년 개정판)](https://www.youtube.com/watch?v=yjuwpf7VH74)
- [코딩애플 React 입문자들이 알아야할 redux 쉽게 설명](https://www.youtube.com/watch?v=QZcYz2NrDIs&t=287s)
- [Flux로의 카툰 안내서](https://bestalign.github.io/translation/cartoon-guide-to-flux/)
- [오픈소스컨설팅 복잡하고 어려운 Redux 적응기](https://tech.osci.kr/2022/06/29/%EB%B3%B5%EC%9E%A1%ED%95%98%EA%B3%A0-%EC%96%B4%EB%A0%A4%EC%9A%B4-redux-%EC%A0%81%EC%9D%91%EA%B8%B0/)
- [Redux의 탄생 배경과 원칙](https://velog.io/@brgndy/Redux-%EC%9D%98-%ED%83%84%EC%83%9D-%EB%B0%B0%EA%B2%BD%EA%B3%BC-%EC%9B%90%EC%B9%99)
- [React : Redux createStore deprecated](https://velog.io/@daymoon_/React-Redux-createStore-deprecated)
- [Redux 공식문서](https://ko.redux.js.org/introduction/getting-started/)