---
keyword : Spring
class : Theory
---

## HTTP 메시지 컨버터 인터페이스
```java
org.springframework.http.converter.HttpMessageConverter
package org.springframework.http.converter;

public interface HttpMessageConverter<T> {

boolean canRead(Class<?> clazz, @Nullable MediaType mediaType);
boolean canWrite(Class<?> clazz, @Nullable MediaType mediaType);

List<MediaType> getSupportedMediaTypes();

T read(Class<? extends T> clazz, HttpInputMessage inputMessage)
throws IOException, HttpMessageNotReadableException;

void write(T t, @Nullable MediaType contentType, HttpOutputMessage 
outputMessage)
throws IOException, HttpMessageNotWritableException;
}
```
- HTTP 메시지 컨버터는 HTTP 요청, HTTP 응답 둘 다 사용된다.
- canRead() , canWrite() : 메시지 컨버터가 해당 클래스, 미디어타입을 지원하는지 체크
- read() , write() : 메시지 컨버터를 통해서 메시지를 읽고 쓰는 기능

## 스프링 부트 기본 메시지 컨버터
```java
(일부 생략)
0 = ByteArrayHttpMessageConverter
1 = StringHttpMessageConverter 
2 = MappingJackson2HttpMessageConverter
```
- 스프링 부트는 다양한 메시지 컨버터를 제공하는데, 대상 클래스 타입과 미디어 타입 둘을 체크해서 사용여부를 결정한다. 만약 만족하지 않으면 다음 메시지 컨버터로 우선순위가 넘어간다.

###### 몇가지 주요한 메시지 컨버터를 알아보자.
- ByteArrayHttpMessageConverter : byte[] 데이터를 처리한다.
	- 클래스 타입: byte[] , 미디어타입: */* ,
	- 요청 예) @RequestBody byte[] data
	- 응답 예) @ResponseBody return byte[] 쓰기 미디어타입 application/octet-stream

- StringHttpMessageConverter : String 문자로 데이터를 처리한다.
	- 클래스 타입: String , 미디어타입: */*
	- 요청 예) @RequestBody String data
	- 응답 예) @ResponseBody return "ok" 쓰기 미디어타입 text/plain

- MappingJackson2HttpMessageConverter : application/json
	- 클래스 타입: 객체 또는 HashMap , 미디어타입 application/json 관련
	- 요청 예) @RequestBody HelloData data
	- 응답 예) @ResponseBody return helloData 쓰기 미디어타입 application/json 관련

###### StringHttpMessageConverter
```java
content-type: application/json
@RequestMapping
void hello(@RequetsBody String data) {}
```

###### MappingJackson2HttpMessageConverter
```java
content-type: application/json
@RequestMapping
void hello(@RequetsBody HelloData data) {}
```

###### ?
```java
content-type: text/html
@RequestMapping
void hello(@RequetsBody HelloData data) {}
```

