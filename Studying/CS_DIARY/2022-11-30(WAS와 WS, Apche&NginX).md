# WS와 WAS

- 웹서버(WS)는 apache나 nginx처럼 클라이언트와 직접 통신하는 서버로, 클라이언트의 요청을 받아서 정적인 데이터를 처리하고 동적인 데이터 처리가 필요하면 WAS에게 처리를 요청하고 처리 결과를 클라이언트에게 응답한다. 
- 웹애플리케이션서버(WAS)는 tomcat처럼 DB에 접근하거나 어떤 로직을 수행해야 하는 동적인 처리가 필요할 때 웹서버의 요청을 받아서 처리한다.
- WAS 또한 WS처럼 정적 리소스를 제공할 수 있지만 굳이 둘을 나누는 이유는, WAS가 정적 리소스까지 모두 처리하게 된다면 중요 애플리케이션 로직이 정적 리소스 처리 때문에 방해를 받을 수도 있고 웹서버가 로드밸런싱 역할을 해서 서버 부하를 줄여줄 수도 있으며 또한 DB정보나 주요 로직들이 WAS에 있기 때문에 WS를 앞단에 둠으로써 보안을 강화할 수도 있기 때문이다.

# Apache vs NginX

- Apache서버는 클라이언트의 요청이 들어올 때마다 프로세스를 생성해서 연결하는 prefork 방식을 따르기 때문에 확장성이 높고 하나의 요청만을 처리하기 때문에 웹서버 자체 내에서 동적 컨텐츠를 처리할 수 있다. 그러나 요청이 있을 때마다 프로세스를 새로 만들기 때문에 메모리 부족현상과 C10K(동시 커넥션이 1만개가 넘어가면 더이상 커넥션을 형성하지 못하는) 문제가 생긴다.
- NginX는 C10K 문제를 해결하기 위한 Event-Driven 구조로 클라이언트에서 온 요청을 Event라고 하고 이 요청을 큐 형식으로 된 저장소에 담아두고고정된 숫자(보통 컨텍스트 스위칭을 최소화 하기 위해 CPU의 코어 개수)의 `worker process` 가 순차적으로 처리하되, 오래 걸리는 작업은 따로 빼서 처리하기 때문에 여러개의 요청을 비동기로 처리할 수 있어서 대규모 트래픽을 감당할 수 있다. 그러나 동적 컨텐츠를 처리하기 위해서는 외부 프로세서로 전달하고 처리결과를 전달받아야 하므로 프로세스 속도가 저하될 수 있다.

---
### 연관자료
- [WS와 WAS는 어떻게 다를까?](https://makemethink.tistory.com/169)
- [WS vs WAS](https://kkyu-coder.tistory.com/168)
- [WS WAS 니가 먼데](https://velog.io/@suran/WS-WAS-%EB%8B%88%EA%B0%80%EB%A8%BC%EB%8D%B0)
- [Apache, nginX 차이](https://bentist.tistory.com/80)
- [Apche, nginX 차이 비교](https://rootkey.tistory.com/143)
- [NginX란](https://ssdragon.tistory.com/60)
- [NginX란, apache와 차이점](https://dallog.github.io/what_is_nginx/)