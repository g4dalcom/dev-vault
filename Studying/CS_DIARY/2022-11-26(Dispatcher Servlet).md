# Spring WEb MVC의 Dispatcher Servlet 동작원리

- 클라이언트의 요청을 Dispatcher Servlet이 최초로 받고 요청 정보를 통해 위임할 컨트롤러를 찾아서 `HandlerMapping` 에 전달한다. 
- `HandlerMapping` 에서 `Contoller`  에 처리를 요청하고 controller가 처리한 요청을 `HandlerAdapter` 에서 ModelAndView 객체로 변환하여 dispatcher servlet 에 전달한다.
- dispatcher servlet은 전달받은 ModelAndView 객체를 이용해 매핑되는 view를 검색한다.
- `ViewResolver` 에서 처리 결과를 view에 전달하고 결과물을 dispatcher servlet에 전달한다.
- dispatcher servlet에서 최종 결과물을 클라이언트에게 응답한다.

---

- Dispatcher Servlet이 호출되면 가장 먼저 `doService()` 메서드가 실행되고 이 메서드는 `doDispatch()` 메서드를 호출한다.
- `doDispatch()` 는 `getHandler()` 를 통해 클라이언트 요청에 해당하는 핸들러를 조회하고, `getHandlerAdapter()` 를 통해 해당 핸들러를 받는다. 
- `getHandlerAdapter()` 를 통해 받은 어댑터의 `handle()` 메서드를 실행하고 `handle()` 메서드는 `processDispatchResult()` 메서드를 실행한다.
- `processDispatchResult()` 는 `render()` 메서드를 호출하고 `render()` 메서드는 `viewResolver` 를 통해 `view` 를 찾아서 전달한다.

---
### 연관자료
- [[Dispatcher-Servlet]]
- [Distpatcher Servlet 동작원리](https://velog.io/@ejung803/Spring-Web-MVC%EC%9D%98-Dispatcher-Servlet%EC%9D%98-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC)