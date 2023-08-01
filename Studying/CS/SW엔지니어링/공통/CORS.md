---
keyword : Security
class : CS
---


![https://beomy.github.io/static/af4eb5de91d11272a3933a4ca158b2a9/f635c/cors.png](https://beomy.github.io/static/af4eb5de91d11272a3933a4ca158b2a9/f635c/cors.png)

# CORS란?

![https://beomy.github.io/assets/img/posts/browser/access-control-allow-origin.png](https://beomy.github.io/assets/img/posts/browser/access-control-allow-origin.png)

위의 그림의 `CORS policy` 오류 메시지는 CORS 정책을 위반할 때 발생하게 됩니다. CORS는 Cross-Origin Resource Sharing의 약자입니다. 교차 출처 리소스 공유로 번역될 수 있는데, 브라우저에서 다른 출처의 리소스를 공유하는 방법입니다.


# URL 구조

다른 출처의 출처가 무엇인지 살펴봐야 하는데, 출처가 무엇인지 알기 위해서 먼저 URL의 구조를 살펴보아야 합니다. URL 구조는 아래 그림과 같습니다.

![https://beomy.github.io/assets/img/posts/browser/url.png](https://beomy.github.io/assets/img/posts/browser/url.png)

프로토콜의 HTTP는 80번, HTTPS는 443번 포트를 사용하는데, 80번과 443번 포트는 생략이 가능합니다.


# 출처(Origin)란?

출처(Origin)란 URL 구조에서 살펴본 Protocal, Host, Port를 합친 것을 말합니다. 브라우저 개발자 도구의 콘솔 창에 `location.origin`를 실행하면 출처를 확인할 수 있습니다.

![https://beomy.github.io/assets/img/posts/browser/location_origin.png](https://beomy.github.io/assets/img/posts/browser/location_origin.png)

# 같은 출처 VS 다른 출처

같은 출처인지 다른 출처인지 이해를 돕기 위해 예제를 하나 살펴보도록 하겠습니다. 현재 웹페이지의 주소가 `https://g4dalcom.github.io/tech/`일 때 같은 출처인지 다른 출처인지 아래 테이블과 같은 결과를 얻을 수 있습니다.

| URL                          | 동일여부 | 이유                     |
|:-----------------------------|:---------|:-------------------------|
| https://g4dalcom.github.io/about | O | Protocal, Host, Port 동일|
|https://g4dalcom.github.io/about?q=work|O|Protocal, Host, Port 동일|
|https://g4dalcom.github.io/about#work | O|Protocal, Host, Port 동일|
|http://g4dalcom.github.io| X |  Protocal 다름|
|https://g4dalcom.github.io:81/about| X |Port 다름|
|https://g4dalcom.heroku.com| X |Host 다름  |

# 동일 출처 정책(Same-Origin Policy)이란?

Postman으로 API를 테스트하거나, 다른 서버에서 API를 호출할 때는 멀쩡히 잘 동작하다가 브라우저에서 API를 호출할 때만 `CORS policy` 오류가 발생해서 당혹스러울 때가 있으셨을 수도 있습니다. 그 이유는 브라우저가 동일 출처 정책(Same-Origin Policy, SOP)를 지켜서 다른 출처의 리소스 접근을 금지하기 때문입니다. 하지만 실제로 웹페이지는 상당히 자주 다른 출처의 리소스를 사용해야 합니다. 예를 들어 `g4dalcom.github.io`라는 도메인 주소를 사용하는 웹페이지에서 `g4dalcom-api.github.io`라는 API 서버로 데이터를 요청해서 화면을 그린다면 이 웹페이지는 동일 출처 정책을 위반한 것이 됩니다.


# 동일 출처 정책의 장점

동일 출처 정책을 지키면 외부 리소스를 가져오지 못해 불편하지만, 동일 출처 정책은 [XSS](https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%ED%8A%B8_%EA%B0%84_%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8C%85)나 [XSRF](https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%ED%8A%B8_%EA%B0%84_%EC%9A%94%EC%B2%AD_%EC%9C%84%EC%A1%B0) 등의 보안 취약점을 노린 공격을 방어할 수 있습니다. 하지만 현실적으로는 외부 리소스를 참고하는 것은 필요하기 때문에 외부 리소스를 가져올 수 있는 방법이 존재해야 합니다. 외부 리소스를 사용하기 위한 SOP의 예외 조항이 CORS입니다.


# CORS 동작원리

CORS의 동작 방식은 단순 요청 방법과 예비 요청을 먼저 보내는 방법 2가지 방법이 있습니다.


# Simple request

단순 요청 방법은 서버에게 바로 요청을 보내는 방법입니다. 아래 그림은 자바스크립트에서 API를 요청할 때 브라우저와 서버의 동작을 나타내는 그림입니다.

![https://beomy.github.io/assets/img/posts/browser/cors_simle_request.png](https://beomy.github.io/assets/img/posts/browser/cors_simle_request.png)

단순 요청은 서버에 API를 요청하고, 서버는 `Access-Control-Allow-Origin` 헤더를 포함한 응답을 브라우저에 보냅니다. 브라우저는 `Access-Control-Allow-Origin` 헤더를 확인해서 CORS 동작을 수행할지 판단합니다.

### Simple request 조건

서버로 전달하는 요청(request)이 아래의 3가지 조건을 만족해야 서버로 전달하는 요청이 단순 요청으로 동작합니다.

-   요청 메서드(method)는 GET, HEAD, POST 중 하나여야 합니다.
-   Accept, Accept-Language, Content-Language, Content-Type, DPR, Downlink, Save-Data, Viewport-Width, Width를 제외한 헤더를 사용하면 안 됩니다.
-   Content-Type 헤더는 application/x-www-form-urlencoded, multipart/form-data, text/plain 중 하나를 사용해야 합니다.

첫 번째 조건은 어렵지 않은 조건이지만 2번, 3번 조건은 까다로운 조건입니다. 2번 조건은 사용자 인증에 사용되는 `Authorization` 헤더도 포함되지 않아 까다로운 조건이며, 3번 조건은 많은 REST API들이 `Content-Type`으로 `application/json`을 사용하기 때문에 지켜지기 어려운 조건입니다.

# Preflight request

Preflight 요청은 서버에 예비 요청을 보내서 안전한지 판단한 후 본 요청을 보내는 방법입니다. 아래 그림은 Preflight 요청 동작을 나타내는 그립입니다.

![https://beomy.github.io/assets/img/posts/browser/cors_preflight_request.png](https://beomy.github.io/assets/img/posts/browser/cors_preflight_request.png)

`GET`, `POST`, `PUT`, `DELETE` 등의 메서드로 API를 요청했는데, 크롬 개발자 도구의 네트워크 탭에 `OPTIONS` 메서드로 요청이 보내지는 것을 보신 적 있으시다면 CORS를 경험하셨던 것입니다. Preflight 요청은 실제 리소스를 요청하기 전에 `OPTIONS`라는 메서드를 통해 실제 요청을 전송할지 판단합니다.

`OPTIONS` 메서드로 서버에 예비 요청을 먼저 보내고, 서버는 이 예비 요청에 대한 응답으로 `Access-Control-Allow-Origin` 헤더를 포함한 응답을 브라우저에 보냅니다. 브라우저는 단순 요청과 동일하게 `Access-Control-Allow-Origin` 헤더를 확인해서 CORS 동작을 수행할지 판단합니다.

# Spring으로 해결하기

## 1. Configuration

이 방법은 Global하게 적용하는 방법입니다.

우선 **config**패키지를 만들어 줍니다. 경로는 **/src/main/java/{project}/config**

만들어진 **config**패키지 안에 **WebConfig**클래스를 만들어줍니다.

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
}
```

클래스 상단에 @Configuration 어노테이션을 통해 설정파일이라는 것을 알려줍니다.

그리고 WebMvcConfigurer를 implements합니다.

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

	@Override
	public void addCorsMappings(CorsRegistry registry) {
	    }
}
```

addCorsMappings메소드를 오버라이드 합니다.

---

# 2.1. addMapping

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**");
    }
}
```

registry.addMapping을 이용해서 CORS를 적용할 URL패턴을 정의할 수 있습니다.

위 처럼 "/**" 와일드 카드를 사용할 수도 있습니다.

또한 Ant-style도 지원하며 "/somePath/**" 이렇게 적용할 수도 있습니다.

Default값은 아래와 같습니다.

-   **Allow all origins.**
-   **Allow "simple" methods GET, HEAD and POST.**
-   **Allow all headers.**
-   **Set max age to 1800 seconds (30 minutes).**

---

# 2.2. allowedOrigins

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("*");
}
```

allowedOrigins 메소드를 이용해서 자원 공유를 허락할 Origin을 지정할 수 있습니다.

위 처럼 "*"로 모든 Origin을 허락할 수 있습니다.

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("<http://localhost:8080>", "<http://localhost:8081>");
    }
}
```

한번에 여러 Origin을 설정할 수 있습니다.

---

# 2.3. allowedMethods

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("*")
                .allowedMethods("GET", "POST");
}@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("*")
                .allowedMethods("GET", "POST")
                .maxAge(3000);
}
```

allowedMethods를 이용해서 허용할 HTTP method를 지정할 수 있습니다.

위 처럼 여러개를 지정할 수 있고 마찬가지로 "*"를 이용하여 모든 method를 허용할 수 있습니다.

---

# 2.4. maxAge

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("*")
                .allowedMethods("GET", "POST")
                .maxAge(3000);
}
```

maxAge메소드를 이용해서 원하는 시간만큼 pre-flight 리퀘스트를 캐싱 해둘 수 있습니다.

---

# 3. Annotation이용하기

두번째 방법은 Controller 또는 메소드단에서 annotation을 통해 적용하는 방법입니다.

```java
@RequestMapping("/somePath")
@CrossOrigin(origins = "*", allowedHeaders = "*")
public class SomeController {
}
```

위처럼 CrossOrigin이라는 어노테이션을 사용하면, global하게 설정하던것과 같이 허용할 origins이나 methods를 지정할 수 있습니다.

origins, methods, maxAge, allowedHeaders를 사용하시면 됩니다.

```java
@RestController
@RequestMapping("/somePath")
public class SomeController {

    @CrossOrigin(origins="*")
    @RequestMapping(value = "/{something}",method = RequestMethod.DELETE)
    public ResponseEntity<String> delete(@PathVariable Long reservationNo) throws Exception{
    }

}
```

또한 메소드에 적용되게 할 수 있습니다.

---
### 관련자료

[출처1](https://beomy.github.io/tech/browser/cors/)

[출처2](https://dev-pengun.tistory.com/entry/Spring-Boot-CORS-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)