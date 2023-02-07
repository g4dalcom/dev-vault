# Deque란?

### 스택(Stack)
- 자료구조의 하나로서 후입선출(Last In First Out)를 의미한다.
- 자바에서는 Stack을 `class 형태`로 지원해주고 있다

### 큐(Queue)
- Queue의 경우는 선입선출(First In First Out)를 의미한다.
- 자바에서 Queue는 `인터페이스로 구현`이 되어 있어 보통 LinkedList를 사용해서 구현하곤 한다.

### 덱, 데크(Deque)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbDHCik%2FbtrYhbDUEA0%2FeC5Ng19lk53l3X4pkjYfuk%2Fimg.png)
- 자바 1.6부터 지원하게 된 Deque는 Queue 인터페이스를 확장하여 만든 `인터페이스`이다.
- 덱은 Double-Ended Queue의 줄임말로 큐의 양쪽에서 데이터를 넣고 뺄 수 있는 자료구조이다. 하나의 자료구조에 Queue와 Stack을 합쳐놓은 형태라고 생각하면 된다.
- 인덱스를 통해 검색, 추가, 삭제가 어렵다.


# Deque 사용법

### 선언

```java
Deque<Type> deque = new ArrayDeque<>();
```

### 데이터 추가

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbJQ6cy%2FbtrYjlswX2U%2FMygJrOE8cIwdWZimPPKdAK%2Fimg.png)

```java
// 앞에 데이터 삽입
deque.addFirst(); // 용량 초과시 Exception
deque.offerFirst(); // 용량 초과시 false

// 뒤에 데이터 삽입
deque.addLast(); // 용량 초과시 Exception
deque.add(); // addLast()와 동일
deque.offerLast(); // 용량 초과시 false
deque.offer(); // offerLast()와 동일

deque.push(); // addFirst()와 동일
deque.pop(); // removeFirst()와 동일
```


### 데이터 제거

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FvzkXY%2FbtrYkKyi8E9%2FR2zPr38oRVbjkzyj9eOiT1%2Fimg.png)

```java
// 앞의 요소 제거
deque.removeFirst(); // 비어있으면 예외
deque.remove(); // removeFirst()와 동일
deque.poll(); // 비어있으면 null 리턴
deque.pollFirst(); // poll()과 동일

// 뒤의 요소 제거
deque.removeLast(); // 비어있으면 예외
deque.pollLast(); // 비어있으면 null 리턴
```


### 데이터 출력

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FEIDL0%2FbtrYjXdE0ZG%2FqGX5ity4B9NhKEThxHFh11%2Fimg.png)

```java
// 첫 번째 요소 확인
deque.getFirst(); // 비어있으면 예외
deque.peekFirst(); // 비어있으면 null 리턴
deque.peek();// peekFirst()와 동일

// 마지막 요소 확인
deque.getLast(); // 비어있으면 예외
deque.peekLast();// 비어있으면 null 리턴

deque.contain(Object o); // Object 인자와 동일한 엘리먼트가 포함되어 있는지 확인
deque.size(); // Deque에 들어있는 엘리먼트의 개수
```


# Stack보다 Deque 사용을 권장하는 이유

- 클래스 vs 인터페이스
	- 자바는 다중 상속을 지원하지 않기 때문에 클래스로 구현되는 Stack보다 인터페이스인 Deque이 객체지향관점에서 더 많은 유연성을 제공한다.
	
- 동기화 메소드 및 성능
	- 쓰레드 안정성을 위한 전통적인 방법인 Vector.class를 상속받는 Stack은 이로 인해 몇 가지 단점이 존재한다.
		- 모든 작업에서 Lock이 걸려 성능 저하
		- 단일 스레드 실행 성능이 저하

- 인덱스 접근
	- Stack은 인덱스로 접근하여 추가, 삭제, 검색이 가능한데 이는 LIFO 규칙을 위반하는 것이다.

---
### 관련 자료
[Java의 Stack 대신 Deque](https://tecoble.techcourse.co.kr/post/2021-05-10-stack-vs-deque/)
[Stack 대신 Deque 사용하기](https://kdhyo98.tistory.com/m/62)