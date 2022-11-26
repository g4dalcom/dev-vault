---
keyword : Controller, Spring MVC
class : CS
---

### Dispatcher Servlet이란 ?

-   `Dispatcher Servlet`에 있는 dispatch는 "보내다"라는 뜻을 가진다.
-   보내다 라는 의미를 포함하고 있는 **`Dispatcher Servlet` 은 HTTP 프로토콜로 들어오는 모든 요청을 가장 먼저 받아 적합한 컨트롤러에 보내주는 `Front Controller`** 이다.
    -   → `Front Controller` 는 주로 서블릿 컨테이너의 제일 앞단에서 서버로 들어오는 클라이언트의 모든 요청을 받아서 처리해주는 컨트롤러 ( MVC 구조에서 함께 사용되는 디자인 패턴 )
-   즉, `Dispatcher Servlet`은 `Front Controller`로서 **클라이언트로 부터 어떠한 요청이 오게 되면 모든 요청을 자신이 먼저 받게 되고, 이러한 요청들을 세부 컨트롤러에게 전달**하는 역할 !
-   한마디로 `Dispatcher Servlet`은 해당 어플리케이션으로 들어오는 모든 요청을 핸들링 해주기 때문에, 우리는 컨트롤러를 구현해두기만 하면 `Dispatcher Servlet`가 알아서 적합한 컨트롤러로 위임을 해주는 구조가 된다.

### Dispatcher Servlet의 동작 과정

![](https://velog.velcdn.com/images/ejung803/post/f97cbbcd-8f4c-440f-83d5-1c41987db16b/image.png)

1.  클라이언트의 요청을 `Dispatcher Servlet`에 전달
2.  요청한 url에 맞는 `controller` 검색하여 `Handler Mapping`에 전달
3.  `HandlerMapping`에서 해당 `controller`에 처리 요청
4.  `controller`에서 처리 결과를 `Handler Adapter`에서 ModelAndView 객체로 변환하여 `Dispatcher Servlet`에 전달
5.  `Dispatcher Servlet`에서 전달받은 ModelAndView 객체를 이용하여 매핑되는 View를 검색
6.  `viewResolver`에서 처리 결과를 `view`에 전달
7.  처리결과가 포함된 `view`를 `Dispatcher Servlet`에 전달
8.  `Dispatcher Servlet`에서 최종 응답 결과를 클라이언트에게 반환

### Dispatcher Servlet의 동작 원리

-   우선 가장 기본적으로 servlet이 호출되면 아래와 같이 `doService()` 메서드가 호출됨
-   `doService()`의 코드는 맨 아래에서 `doDispatch()` 메서드가 호출됨  
    → 이제 실제로 dispatch(보내다~!)를 실행하는 과정이 진행됨  
    ![](https://blog.kakaocdn.net/dn/NDpIg/btrj7Rwdv8k/PbRUk5x2N8RGBwt8gO1ojk/img.png)

#### 1. 핸들러 조회, 핸들러 어댑터 조회

-   `doDispatch()`에서는 클라이언트의 요청에 해당하는 핸들러를 조회하고(`getHandler()`),  
    해당 핸들러를 받기 위한 어댑터를 조회(`getHandlerAdapter()`)
-   (과정에서의 2,3,4 번 진행을 위한 단계)  
    ![](https://blog.kakaocdn.net/dn/1AlsS/btrj7RJJJwV/Vj0eYbKMXDkXRjcVxKTMV0/img.png)

#### 2. 위에서 찾은 핸들러 어댑터의 `handle()` 메서드를 호출

-   이를 통해 ModelAndView를 반환
-   그리고 아래에서 `processDispatchResult()` 메서드를 호출
-   (과정에서의 2,3,4 번 진행)  
    ![](https://blog.kakaocdn.net/dn/0fqCl/btrj885xKDP/s9TIoi1dasy65yILrJzvx1/img.png)

#### 3. 뷰 렌더링 호출

-   `processDispatchResult()` 메서드는 안에서 `render()` 메서드를 또 다시 호출
-   (과정에서의 5,6,7,8 번 진행을 위한 단계)  
    ![](https://blog.kakaocdn.net/dn/HGwlB/btrj87Mkmez/ZXZLKyDXQo6eCeLsxMtk4K/img.png)

#### 4. 뷰 렌더링

-   `render()` 메서드 안에서는 뷰 리졸버(viewResolver)를 통해 view를 찾고, 해당 view를 반환받음
-   마지막으로 뷰를 실제로 렌더링 → 전달
-   (과정에서의 5,6,7,8 번 진행)  
    ![](https://blog.kakaocdn.net/dn/QZtg0/btrj2AiEFZy/5Ldj2sSOMMRsMsx96FFVf1/img.png)


- [[Spring] SpringBoot 소스 코드 분석하기, DispatcherServlet(디스패처 서블릿) 동작 과정 - (7)](https://mangkyu.tistory.com/216)
- [Distpatcher Servlet 동작원리](https://velog.io/@ejung803/Spring-Web-MVC%EC%9D%98-Dispatcher-Servlet%EC%9D%98-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC)