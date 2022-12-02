# TCP/IP

- 데이터를 작게 잘라서 전송하는 방식을 패킷이라고 하는데, IP와 TCP는 이러한 패킷 통신을 위한 규약이다.
- IP는 패킷 데이터들을 최대한 빨리 목적지로 전송하는 것이 목적이기 때문에 패킷이 제대로 전달되었는지나 패킷이 순서대로 전달 되었는지를 보장하지 않는다.
- TCP는 3 way handshaking을 통해 데이터 전달을 보장하고 데이터 조각을 순서대로 정렬해서 빠진 조각들은 재요청한다.
- TCP/IP는 두 가지 프로토콜 방식을 조합해서 IP를 이용해 데이터를 빠르게 전송하고 TCP가 데이터의 신뢰성을 보장하는 방식이다.

# Docker와 VM 차이
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F9A97O%2FbtrSDGargKA%2FtvlC8I1MzcMs6uESyInkX1%2Fimg.jpg)
- VM과 Docker는 둘 다 기본 하드웨어에서 격리된 환경으로 어플리케이션을 구동한다.
- VM은 하이퍼바이저 기술을 통해서 Host OS 위에 여러 개의 Guest OS를 사용하는 방법이며 각각 운영체제를 포함하고 있어서 무겁지만 완전히 독립된 공간과 자원을 할당 받기 때문에 보안성이 높고 여러 OS를 가상화할 수 있다.
- Docker는 도커엔진이라는 하나의 리눅스 OS를 갖는 VM이 있고 그 위에 컨테이너라는 격리된 서비스 환경을 만들어 운영하는 방식으로 상대적으로 가벼우며 Docker가 설치된 환경이라면 같은 개발환경으로 어디서든 사용 가능하다.

---
### 연관자료
- [TCP/IP 4계층 총정리](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-TCP-IP-%EC%A0%95%EB%A6%AC-%F0%9F%91%AB%F0%9F%8F%BD-TCP-IP-4%EA%B3%84%EC%B8%B5)
- [개알못을 위한 TCP/IP 개념](https://brunch.co.kr/@wangho/6)
- [물 흐르듯 읽어보는 TCP/IP](https://velog.io/@haero_kim/%EB%AC%BC-%ED%9D%90%EB%A5%B4%EB%93%AF-%EC%9D%BD%EC%96%B4%EB%B3%B4%EB%8A%94-TCPIP)
- [TCP/IP 4계층이란?](https://wooono.tistory.com/507)
- [Docker와 VM의 차이점](https://hu-nie.tistory.com/entry/Docker-%EC%99%80-VM%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90)
- [도커와 가상머신](https://hoon93.tistory.com/41)
- [Docker와 VM](https://medium.com/@darkrasid/docker%EC%99%80-vm-d95d60e56fdd)
- [도커와 VM의 차이](https://velog.io/@kdaeyeop/%EB%8F%84%EC%BB%A4-Docker-%EC%99%80-VM%EC%9D%98-%EC%B0%A8%EC%9D%B4)