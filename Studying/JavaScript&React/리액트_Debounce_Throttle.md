# 개념

![](https://blog.kakaocdn.net/dn/c1UEto/btsogbZAkjQ/hgKSeC68vaHFv8sG1GqAuk/img.gif)

- input 타입에 onChange 이벤트를 걸어두었다면 사용자가 input창에 무언가를 입력할 때마다 이벤트 핸들러가 호출됩니다.
- 만약에 검색 기능을 구현하고자 한다면 사용자가 검색 버튼을 누를 때의 value만 알면 되기 때문에 그 이전의 호출들은 리소스 낭비만 되는 셈이죠!

![](https://blog.kakaocdn.net/dn/bzAWOL/btsohYL61cJ/BhE8YXZXPHNmQuQH8e2K9K/img.gif)

- 그런데 사용자가 검색 중에도 연관된 검색어를 띄어주고 싶다면 사용자의 입력마다 데이터 요청이 있어야 하므로 이때는 매번의 호출이 필요한 요청이 됩니다.

![](https://blog.kakaocdn.net/dn/L1DYE/btsomP8KQ78/F2vo0B5hQ6wqSleqekuFL0/img.gif)

- scroll event의 경우도 마찬가지입니다! 만약 무한 스크롤 기능을 스크롤 이벤트로 구현한다면 스크롤이 맨 아래에 위치했는지 여부만 체크하면 되는데 스크롤을 움직일 때마다 이벤트가 연속적으로 호출되겠죠! 
- **Debounce**와 **Throttle**은 이러한 연속적으로 호출되는 이벤트 핸들러의 실행 빈도를 제어함으로써 성능상의 유리함을 가져오기 위한 개념입니다.

## Debounce

- **연이어 발생한 이벤트를 하나의 그룹으로 묶은 후 특정 시간이 지난 후 하나의 이벤트만 발생하도록 하는 기술** 입니다.
- 주로 가장 마지막 호출이나 처음 호출만 유효하도록 합니다.

![](https://blog.kakaocdn.net/dn/cbwWo2/btsomXeQwm9/uczQpnWKW6MukkDj7Jpmb0/img.gif)

- 위는 실제 키입력이고 아래는 이벤트 발생을 나타낸 것인데요!
- 일정시간 안에 연속적인 클릭이 일어나면 이벤트를 발생시키지 않다가 **특정 시간이 지나도 더이상 호출이 발생하지 않으면** 그때서야 이벤트를 발생시킵니다.
- 위에서 잠깐 언급했던 input창의 onChange 이벤트나 스크롤 이벤트, 창 리사이즈 이벤트 등에서 필요한 마지막 값만을 추적하면서 리소스를 아낄 수 있겠네요!


[제로초](https://www.zerocho.com/category/JavaScript/post/59a8e9cb15ac0000182794fa)
https://pks2974.medium.com/throttle-%EC%99%80-debounce-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC%ED%95%98%EA%B8%B0-2335a9c426ff

https://velog.io/@sunhwa508/%ED%98%BC%EB%9E%80%ED%95%9C-%EB%94%94%EB%B0%94%EC%9A%B4%EC%8A%A4debounce%EC%99%80-%EC%8A%A4%EB%A1%9C%ED%8B%80throttle-%EC%B0%A8%EC%9D%B4

https://the-dev.tistory.com/88


https://webclub.tistory.com/607


https://onlydev.tistory.com/151