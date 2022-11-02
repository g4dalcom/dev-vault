# Queue란?

Queue란 Collection 프레임워크의 일부이며 java.util 패키지에 소속

![](https://k.kakaocdn.net/dn/4xNC4/btq5pEMF0sO/Hz54KOz8oU8QwR8uCqyIMK/img.png)

Queue는 사전적으로 "줄을 서다"를 의미
줄을 서서 기다린다는 것처럼 먼저 들어오면 데이터가 먼저 나가는 형식

일명 FIFO(FirstInFirstOut) 방식(선입선출)
반대로 Stack은 LIFO방식이라 두 개가 많이 비교됨(후입선출)

# Queue 선언과 사용

###### 선언
```java
Queue<Integer> queue = new LinkedList<>();
```

###### 삽입
```java
Queue<Integer> queue = new LinkedList<>();

queue.add(1);
queue.add(2);
queue.offer(3);
```
-   add(e) : 삽입 성공 시 true 반환, 하지만 사용 가능한 공간이 없어 삽입 실패 시 IllegalStateException 발생
-   offer(e) : 삽입 성공 시 true 반환, 하지만 사용 가능한 공간이 없어 삽입 실패 시 false 반환

###### 삭제
```java
Queue<Integer> queue = new LinkedList<>();

queue.add(1);
queue.add(2);
queue.offer(3);

queue.poll();
queue.remove();
```
-   remove() : 헤드 요소를 조회(출력 가능)하고 제거, 하지만 큐가 비어 있다면 예외 발생
-   poll() : 헤드 요소를 조회(출력 가능)하고 제거, 하지만 큐가 비어 있다면 null 반환

###### 헤드 조회
```java
Queue<Integer> queue = new LinkedList<>();

queue.add(1);
queue.add(2);

queue.peek();
```
-   element() : 헤드 요소 조회 및 반환, 하지만 큐가 비어 있다면 예외 발생
-   peek() : 헤드 요소 조회 및 반환, 하지만 큐가 비어 있다면 null 반환