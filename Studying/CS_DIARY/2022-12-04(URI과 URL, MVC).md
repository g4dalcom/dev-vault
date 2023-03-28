# URI와 URL 차이

- URI는 네트워크상에서 자원을 식별하기 위한 문자열의 구성이고
- URL은 네트워크상에서 자원이 어디에 있는지 알려주기 위한 주소이다.
- https://www.google.com 의 경우 URI 이면서 URL이다.
- https://www.google.com/images/cat.png 의 경우 cat.png 라는 자원의 위치를 가리키므로 URI이면서 URL이다.
- https://www.google.com/user?id=hello 의 경우 https://www.google.com/user 까지는 유저라는 자원의 위치를 가리키므로 URI이자 URL이지만 쿼리스트링으로 붙은 id=hello 부분은 자원 중에서도 식별하기 위한 문자열이므로 URI라고만 할 수 있다.

##### 참고 (URL과 URI 구성)
- query, fragment(#)는 URI에만 해당함
| 부분                             | 명칭     | 설명                                                                                |
|:-------------------------------- |:-------- |:----------------------------------------------------------------------------------- |
| file://, http://, https://       | scheme   | 통신 프로토콜                                                                       |
| 127.0.0.1, www.google.com        | hosts    | 웹 페이지, 이미지, 동영상 등의 파일이 위치한 웹 서버, 도메인 또는 IP                |
| :80, :443, :3000                 | port     | 웹 서버에 접속하기 위한 통로                                                        |
| /search, /Users/username/Desktop | url-path | 웹 서버의 루트 디렉토리로부터 웹 페이지, 이미지, 동영상 등의 파일이 위치까지의 경로 |
| ?=Javascript                     | query    | 웹 서버에 전달하기 위한 추가 질문                                                                                    |



# MVC 패턴

- 애플리케이션을 Model - View - Controller 라는 세 가지 역할로 구분해서 비즈니스 로직과 인터페이스를 분리한 디자인 패턴이다. 어느 한 쪽이 변경되어도 다른 쪽은 영향을 받지 않기 때문에 유지보수에 용이하다.
- (MVC model2) 사용자는 모든 요청을 Controller(Servlet)를 거치게 되고 Controller는 비즈니스 로직을 담당하는 Model(JavaBeans)이 처리한 요청을 받아서 사용자에게 보여줄 View를 선택한다. View(JSP)는 Controller에게 Model이 처리한 데이터를 전달 받아서 화면에 입힌 후 사용자에게 보여준다.
- MVC 패턴은 프로젝트 규모가 커질 수록 Controller에서 공통으로 처리해야 하는 부분이 증가한다는 한계점이 있는데 이를 보완한 것이 FrontController(DispatcherServlet)을 이용한 스프링MVC이다.

##### 참고
- MVC model1은 JSP가 Controller와 View 역할을 모두 담당하여 클라이언트의 요청도 받고 Model과 상호작용도 하고 View도 그려낸다. 흐름이 단순하기 때문에 개발이 쉽다는 장점이 있지만 프로젝트 규모가 커지면 유지보수가 어려워지고 하나의 JSP에 Controller와 View가 모두 있기 때문에 디자이너와 개발자의 협업이 어렵다.

---
### 연관자료
- [URL / URI / URN 차이점 - 한방 이해하기](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-URL-URI-%EC%B0%A8%EC%9D%B4)
- [URL 과 URI 차이점](https://millo-l.github.io/URI%EC%99%80-URL%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90/)
- [MVC 패턴이란](https://cocoon1787.tistory.com/733)
- [Model 1 아키텍처와 Model 2 아키텍처, MVC 패턴](https://scshim.tistory.com/272)