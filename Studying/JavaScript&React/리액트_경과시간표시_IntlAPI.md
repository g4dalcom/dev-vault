# 결과 화면

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdZlqD2%2FbtsmGBepSso%2FyJgSKVNohH1MLWuHIhzcQk%2Fimg.png)


# 개요

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbPIqZ6%2FbtsmIhMNaq8%2FLiZchItx8KjpzfLQJqTeGk%2Fimg.png)

- 매우 간단한 댓글 컴포넌트를 만들어보았습니다.
- 백엔드 데이터로 작성일자를 받으면 위와 같은 **ISO 형식**으로 받게됩니다.
- 처음 피그마로 작업할 때는 **2023-07-06 12:23** 의 형식으로 편집하여 출력하려고 하였으나 핀터레스트, 트위터, 인스타그램 등 유명 SNS의 댓글 UI를 보면 좀 더 직관적으로 날짜를 표시해주는 것을 알게 되었습니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fkz6kM%2FbtsmKz0bETe%2FB2h8qWGgXnRJU2AIF7h1u1%2Fimg.png)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F5v2cS%2FbtsmHagkyL2%2FRTkvbf1fnAEQvgsyPyjlh1%2Fimg.png)

- **dayjs**나 **moment**와 같은 날짜를 다루기 쉽게 도와주는 라이브러리도 있지만 최대한 직접 기능을 구현해보고 싶어서 라이브러리는 이용하지 않았습니다.

## 구현

```typescript
const elapsedTime = (date: Date) => {
  const start = new Date(date);
  const end = new Date();

  const diff = (end.getTime() - start.getTime()) / 1000;
```

- date를 매개변수로 받는 함수입니다. date는 사용하는 곳에서의 작성일자 혹은 수정일자가 들어가겠죠!
- 대상이 되는 날짜를 **start** 변수에 담고 현재 날짜는 **end** 에 담아서 둘의 차이를 계산합니다. 1000으로 나누어주는 이유는 Date()값은 기본적으로 milliseconds로 나오기 때문입니다.
- 자바스크립트의 경우는 end - start를 하면 알아서 number로 형변환한 후 계산을 해주지만
- 타입스크립트는 start와 end의 타입을 **Date**로 알고 있기 때문에 뺄셈 연산을 하지 못한다고 오류를 뱉어냅니다.
- 따라서 `end.getTime()` 혹은 `Number(end)` 혹은 `+end` 로 형변환을 명시적으로 해주어야 합니다!

```typescript
  const times = [
    { name: "년", milliSeconds: 60 * 60 * 24 * 365 },
    { name: "개월", milliSeconds: 60 * 60 * 24 * 30 },
    { name: "일", milliSeconds: 60 * 60 * 24 },
    { name: "시간", milliSeconds: 60 * 60 },
    { name: "분", milliSeconds: 60 },
  ];

  for (const value of times) {
    const betweenTime = Math.floor(diff / value.milliSeconds);

    if (betweenTime > 0) {
      return `${betweenTime}${value.name} 전`;
    }
  }

  return "방금 전";
```

- 대상 시간과 현재 시간의 차이를 구했다면 그 차이에 따라 적절한 문구를 출력해주어야 합니다.
- **년 ~ 분** 의 순서로 탐색을 하며 둘의 차이가 나는 경우에 해당 name을 출력해주게 됩니다.
 - 경과시간에 milliSeconds를 나눠서 해당 값보다 경과시간이 적을 시, 0보다 작은 소수점 값이 나오게 됩니다. ( 1분 / 1년 ) => 0.0000019...
- 정수가 나올 때까지(0보다 큰 값) 다음 단위로 넘어가서 다시 비교를 반복합니다. 모든 탐색이 끝났음에도 return 되지 않았다는 것은 두 날짜의 차이가 1분보다 작다는 의미이므로 "방금 전"이라는 문자를 반환하도록 하였습니다.


## Intl API 이용해보기

> Intl API의 주요 내용을 정리하기 위해 본문과 상관없는 내용을 정리해두었습니다. 위 내용과 이어지는 부분이 궁금하시다면 아래쪽에 **Intl.RelativeTimeFormat** 부분부터 봐주세요!

### Intl API란?

- 웹 서비스를 여러가지 언어로 설계하고 구현하는 과정을 **국제화(Internalization)** 라고 합니다. 단순히 문구를 번역하는 것뿐만 아니라 동일한 데이터를 다른 형식으로 보여주어야 하기도 합니다.
- 예를 들어서 날짜의 경우 한국에서는 연, 월, 일, 요일 순서를 따르지만 영어권에서는 요일, 월, 일, 년 순서를 따릅니다. 시간을 표시할 때도 한국에서는 오전, 오후를 제일 앞에 명시하는 반면 영어에서는 보통 제일 뒤에 명시한다는 차이가 있습니다.
- 이러한 작업을 일일이 하려면 굉장한 노력이 필요하고 외부 라이브러리를 사용한다면 번들 사이즈에 영향이 갈 수밖에 없겠네요!
- 이러한 문제점을 해결하기 위해 자바스크립트에서는 **Intl API**를 제공하고 있습니다. 이 API는 대부분의 모던 브라우저에서 지원되며 Node.js에서도 사용이 가능합니다.

### Intl API 사용법

- Intl API는 전역에서 접근이 가능한 **Intl** 객체를 통해 사용이 가능합니다. 
- 모든 생성자는 공통적으로 2가지 인자를 받는데 첫 번째 인자는 **locale** 이라는 언어와 지역 정보를 표준화 시킨 코드이며 두 번째 인자는 추가적인 **option** 을 명시할 수 있습니다.

#### 예시 코드

- 현재 날짜를 한국 이용자들을 위한 형식으로 변환하려고 한다면 `DateTimeFormat()` 생성자로 생성한 객체의 `format()` 함수를 호출하면 됩니다.

```javascript
const koDtf = new Intl.DateTimeFormat("ko", { dateStyle: "long" });

kodtf.format(new Date());

// '2023년 7월 7일'
```

### 주요  생성자

#### Intl.DateTimeFormat

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcXOQ8k%2FbtsmMICAnY2%2F435QCrWNgqde8kZ6pf5CL1%2Fimg.png)

- 시간이나 날짜를 쉽게 국제화할 수 있는 생성자입니다.
- 포매팅 옵션으로 **dateStyle** 과 **timeStyle** 이 자주 사용되며 **full, long, medium, short** 을 옵션으로 설정할 수 있습니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb7bPjY%2FbtsmHHEUjLy%2F6tA8uJBO4IPK5b1eNqjG40%2Fimg.png)

- **locale** 속성에 따라 다른 형식으로 출력되는 것을 볼 수 있습니다!

#### Intl.NumberFormat

- 통화(currency), 백분율, 무게, 길이, 속도, 온도 등 **단위가 있는 숫자 데이터를** 다룰 때 유용합니다.
- **style** 옵션을 통해 다양한 스타일을 지원하는데 예를 들어 백분율 데이터를 다룰 때는 **style: 'percent'** 로 설정하고 format() 함수에 소수를 넘겨주는 형식으로 사용이 가능합니다.

```javascript
new Intl.NumberFormat('ko', { style: 'percent' }).format(0.7)

// '70%'
```

- 통화 데이터를 다룰 때는 **style: 'currency'** 로 설정을 하고 옵션으로 통화 코드를 넘겨줍니다.

```javascript
new Intl.NumberFormat('ko', { style: 'currency', currency: 'USD' }).format(40.56)

// 'US$40.56'
```

- 그 외에 다른 단위를 사용할 때는 **style: 'unit'** 으로 주고 옵션으로 단위 코드를 넘겨주는 방식으로 사용할 수 있습니다.

```javascript 
new Intl.NumberFormat('ko', { style: 'unit', unit: 'kilogram' }).format(50)

// '50kg'

new Intl.NumberFormat('ko', { style: 'unit', unit: 'pound' }).format(110)

// '110lb'
```


#### Intl.PluralRules

- 영어에서는 한국어와 다르게 명사의 단복수 개념이 매우 중요합니다.
- 예를 들어서 한국에서는 **한 사람, 두 사람** 이라고 표현한다면 영어에서는 **One person, Two people** 로 명확하게 구분을 합니다. 또한 숫자 0은 복수로 취급이 된다는 특징도 있죠!
- 영어와 같이 단복수를 엄격하게 구분하는 언어로 서비스를 한다면 매우 유용하게 사용할 수 있을 것 같습니다.
- **PluralRules()** 생성자로 만든 객체의 **select()** 함수에 숫자를 넘기면 'one' 또는 'other'를 반환하는데 이 결과값을 가지고 단수와 복수를 구분하게 됩니다.

```javascript
let pr = new Intl.PluralRules("en")

pr.select(0)
'other'

pr.select(1)
'one'

pr.select(2)
'other'

pr.select(3)
'other'

```

- 또한 **type option** 을 기본값 **cardinal** 에서 **ordinal**로 변경을 하면 1st, 2nd, 3rd, 4th 와 같은 서수의 어미 처리를 할 수 있습니다.

```javascript
let pr = new Intl.PluralRules("en", { type: 'ordinal' })

pr.select(0)
'other'

pr.select(1)
'one'

pr.select(2)
'two'

pr.select(3)
'few'

pr.select(4)
'other'
```

- 'one'이 반환되면 어미로 'st'를 사용하고(1st) 'two'가 반환되면 'nd'(2nd), 'few' 가 반환되면 'rd'(3rd), 그 외 'other'이 반환되면 'th'(4th)를 어미로 사용하게 됩니다.

#### Intl.RelativeTimeFormat

- 현재 시간 기준으로 얼마나 시간이 지났는지, 혹은 얼마나 걸릴 것인지를 상대적으로 표현해줍니다.
- **numeric** 옵션을 **auto** 혹은 **always** 로 설정할 수 있는데 **auto**인 경우는 숫자없이 표기할 수 있는 부분은 최대한 문자를 사용해서 표현해줍니다.

###### numeric: 'auto' 

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbkEw2P%2FbtsmHiL8CQ3%2FgJbqhd7mHvQXKZmShSRVHK%2Fimg.png)

###### numeric: 'always'

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcByfwX%2FbtsmJDPnn0D%2FBy3HnnprTQTpjZYukYlxxK%2Fimg.png)


### 구현

```typescript
const formatter = new Intl.RelativeTimeFormat("ko", {
    numeric: "always",
  });

  const times: TimesProps[] = [
    { name: "year", milliSeconds: 60 * 60 * 24 * 365 },
    { name: "month", milliSeconds: 60 * 60 * 24 * 30 },
    { name: "day", milliSeconds: 60 * 60 * 24 },
    { name: "hour", milliSeconds: 60 * 60 },
    { name: "minute", milliSeconds: 60 },
  ];
```

- Intl.RelativeTimeFormat() 을 생성해주고 times의 name값들을 API가 지원하는 형식으로 바꾸어줍니다. 해당 형식은 생성자를 타고 들어가서 내부를 들어가보면 다음과 같이 알려줍니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb5sMYC%2FbtsmHHybJAQ%2FJh7tK4kd5jRzpV6OwSnkH0%2Fimg.png)



![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcyJBOc%2FbtsmIf2vFtC%2F4LX03Fj8mT06tccfO9diGK%2Fimg.png)

- 그리고 format()을 호출하려고 하면 name의 타입이 맞지 않다며 오류를 내버립니다.
- format()은 두 번째 인자로 unit을 받기 때문에 그에 맞는 타입을 적용시켜주어야 합니다.

```typescript
interface TimesProps {
  name: Intl.RelativeTimeFormatUnit;
  milliSeconds: number;
}
```

- 따라서 위와 같이 인터페이스를 선언하고 times에 배열로 적용시켜주면 됩니다!

### 전체 코드

```typescript
interface TimesProps {
  name: Intl.RelativeTimeFormatUnit;
  milliSeconds: number;
}

const elapsedTime = (date: Date) => {
  const start = new Date(date);
  const end = new Date();

  const diff = (end.getTime() - start.getTime()) / 1000;

  const formatter = new Intl.RelativeTimeFormat("ko", {
    numeric: "always",
  });

  const times: TimesProps[] = [
    { name: "year", milliSeconds: 60 * 60 * 24 * 365 },
    { name: "month", milliSeconds: 60 * 60 * 24 * 30 },
    { name: "day", milliSeconds: 60 * 60 * 24 },
    { name: "hour", milliSeconds: 60 * 60 },
    { name: "minute", milliSeconds: 60 },
  ];

  for (const value of times) {
    const betweenTime = Math.floor(diff / value.milliSeconds);

    if (betweenTime > 0) {
      return formatter.format(-betweenTime, value.name);
    }
  }
  return "방금 전";
};

export default elapsedTime;
```

