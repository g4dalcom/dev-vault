# 개념

![](https://blog.kakaocdn.net/dn/c1UEto/btsogbZAkjQ/hgKSeC68vaHFv8sG1GqAuk/img.gif)

- input 타입에 onChange 이벤트를 걸어서 state의 변화를 준다면 사용자가 input창에 무언가를 입력할 때마다 이벤트 핸들러가 호출됩니다.

![](https://blog.kakaocdn.net/dn/L1DYE/btsomP8KQ78/F2vo0B5hQ6wqSleqekuFL0/img.gif)

- scroll event의 경우도 마찬가지입니다. 만약 무한 스크롤 기능을 스크롤 이벤트로 구현한다면 스크롤이 맨 아래에 위치했는지 여부만 체크하면 되는데 스크롤을 움직일 때마다 이벤트가 연속적으로 호출됩니다.
- 댓글을 작성해서 submit 하는 것이나 스크롤이 맨 아래에 위치했는지 여부를 확인해야 하는 경우에 필요한 것보다 훨씬 많은 이벤트 호출이 발생하게 됩니다.
- **Debounce**와 **Throttle**은 이러한 연속적으로 호출되는 이벤트 핸들러의 실행 빈도를 제어함으로써 성능상의 유리함을 가져오기 위한 개념입니다.

## Debounce

- **연이어 발생한 이벤트를 하나의 그룹으로 묶은 후 특정 시간이 지난 후 하나의 이벤트만 발생하도록 하는 기술** 입니다.
- 주로 가장 마지막 호출이나 처음 호출만 유효하도록 합니다.

![](https://blog.kakaocdn.net/dn/cbwWo2/btsomXeQwm9/uczQpnWKW6MukkDj7Jpmb0/img.gif)

- 위는 실제 키입력이고 아래는 이벤트 발생을 나타낸 것인데요!
- 일정시간 안에 연속적인 클릭이 일어나면 이벤트를 발생시키지 않다가 **특정 시간이 지나도 더이상 호출이 발생하지 않으면** 그때서야 이벤트를 발생시킵니다.
- 리사이즈 이벤트와 같이 마지막 액션에 대한 처리가 중요한 경우에 필요한 마지막 값만을 추적하면서 리소스를 아낄 수 있겠네요!

### Debounce 구현

```typescript
import { useRef } from 'react';

function useDebounce<T extends any[]>(
  callback: (...params: T) => void,
  time: number,
) {
  const timer = useRef<ReturnType<typeof setTimeout> | null>(null);
  return (...params: T) => {
    if (timer.current) clearTimeout(timer.current);

    timer.current = setTimeout(() => {
      callback(...params);
      timer.current = null;
    }, time);
  };
}

export default useDebounce;
```

- Debounce는 연속적인 이벤트 호출이 일어나면 이벤트를 발생시키지 않으므로 매번 timer를 설정해서 관리합니다.
- 이미 지정된 timer가 있다면 clearTimeout을 통해 제거하고 새롭게 timer를 세팅하는 것입니다. 이벤트 호출이 일어날 때마다 새로운 timer가 지정되는 것이죠!

## Throttle

- **이벤트를 일정 주기마다 발생하도록 하는 기술**입니다.
- 이벤트 주기를 1초라고 하면 1초 안에는 단 한 번의 이벤트만 발생하도록 조절하는 것이죠!

![](https://blog.kakaocdn.net/dn/bzAWOL/btsohYL61cJ/BhE8YXZXPHNmQuQH8e2K9K/img.gif)

- Throttle은 자동완성이나 무한 스크롤 기능에서 유용하게 사용될 수 있습니다. 계속해서 타이핑을 하거나 스크롤링을 해도 일정 주기마다 이벤트가 호출되므로 처리량은 조절하면서 해당 기능이 계속해서 동작하고 있다고 느끼게 할 수 있습니다.

### Throttle 구현

```typescript
import { useRef } from 'react';

function useThrottle<T extends any[]>(
  callback: (...params: T) => void,
  time: number,
) {
  const timer = useRef<ReturnType<typeof setTimeout> | null>(null);

  return (...params: T) => {
    if (!timer.current) {
      timer.current = setTimeout(() => {
        callback(...params);
        timer.current = null;
      }, time);
    }
  };
}

export default useThrottle;
```

- Throttle은 이미 timer가 설정되어 있다면 아무 동작도 하지 않고 timer가 없다면(최초 호출) timer를 설정하게 됩니다.
- 그러면 이벤트 호출이 계속되는 동안 지정한 time마다 이벤트가 발생할 것입니다!

## 정리

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbRxLYh%2FbtsrErQB9EV%2FXmZgXKMT9UHl29FPBxBHn0%2Fimg.png)

- **디바운싱**은 이벤트 호출이 더이상 발생하지 않을 때 특정 시간이 지나면 이벤트가 발생하며 리사이징 이벤트처럼 마지막 액션의 값이 중요할 때 사용하고
- **쓰로틀링**은 일정한 주기마다 이벤트를 발생시키며 자동완성이나 무한 스크롤 기능에서 유용하게 사용할 수 있습니다!

---
### 관련 자료

- [제로초-쓰로틀링과 디바운싱](https://www.zerocho.com/category/JavaScript/post/59a8e9cb15ac0000182794fa)
- [Throttle 와 Debounce 개념 정리하기](https://pks2974.medium.com/throttle-%EC%99%80-debounce-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC%ED%95%98%EA%B8%B0-2335a9c426ff)
- [혼란한 디바운스(debounce)와 스로틀(throttle) 차이](https://velog.io/@sunhwa508/%ED%98%BC%EB%9E%80%ED%95%9C-%EB%94%94%EB%B0%94%EC%9A%B4%EC%8A%A4debounce%EC%99%80-%EC%8A%A4%EB%A1%9C%ED%8B%80throttle-%EC%B0%A8%EC%9D%B4)
- [Debounce(디바운스) vs Throttle(쓰로틀)](https://the-dev.tistory.com/88)
- [디바운스(Debounce)와 스로틀(Throttle ) 그리고 차이점](https://webclub.tistory.com/607)]
- [디바운싱과 쓰로틀링](https://onlydev.tistory.com/151)