---
keyword : Spring
class : ERROR
---


### API POST 전송 오류


#### 에러내용

- 회원가입 기능 구현 중, 양식을 작성하고 회원가입 버튼을 누르면 HTTP 500 에러가 뜨는 문제

POST : [localhost:8080/user/signup](http://localhost:8080/user/signup) 에서 오류가 발생함

콘솔창에는 Caused by: org.h2.jdbc.JdbcSQLSyntaxErrorException: Syntax error in SQL statement “ 라는 에러메시지 발생

---

#### 원인

- User라는 클래스를 만들어서 유저 회원가입 및 로그인에 대한 기능을 구현하고 있었는데 User라는 단어가 SQL의 키워드 중 하나이기 때문인 것 같다.


---

#### 해결

-   5시간 넘게 별의별 짓은 다 해봄….
-   POST 에서 넘어가는 것이 문제인 것 같아서 컨트롤러의 url 주소와 파라미터 부분에서 잘못된 게 없는지 보았는데 원인을 알 수가 없었음 (애초에 코드스니펫 복붙인데)
-   혹시나 클래스이름이나 코드에 오타가 없는지 확인해보았고 HTTP Message를 확인하면서 어노테이션이나 리턴값이 잘못된 것이 없는지도 확인함

[Controller & HTTP Message](https://www.notion.so/Controller-HTTP-Message-1b13e91b5faf4fdab5209ab4bfc85324)

-   어제는 콘솔창에 다른 에러가 있었던 것 같은데 오늘 확인해보니 Caused by: org.h2.jdbc.JdbcSQLSyntaxErrorException: Syntax error in SQL statement 라는 메시지가 떠서 SQL Syntax 문제라고 생각하고 검색을 해보았더니 Group 이라는 클래스를 만들어서 썼던 분이 나와 비슷한 문제를 겪고 있었음
-   결국 User 클래스 위에 @Table(name = “User_table”) 이라고 어노테이션을 넣어서 DB에 저장할 때 테이블 이름을 User가 아닌 User_table로 저장이 되게 하였더니 해결이 되었음…..
-   오류가 떴을 때 개발자도구와 특히 콘솔창…을 제대로 확인하고 찾아야겠다고 생각함 괜히 이상한 데서 시간을 너무 많이 써버렸다 ㅠㅠ
-   근데 왜 강의에서는 잘 되고 나는 오류가 나는 걸까?….

