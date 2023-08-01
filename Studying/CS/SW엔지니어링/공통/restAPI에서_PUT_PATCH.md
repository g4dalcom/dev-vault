---
keyword : Java
class : CS, Programming
---


### restAPI 에서 PUT과 PATCH

PATCH와 PUT은 둘 다 데이터의 수정을 위한 method이다.
그렇다면 두가지는 어떤 차이점이 있을까?

-   PATCH : which is used to apply partial modifications to a resource
-   PUT : method requests that the state of the target resource be created or replaced with the state defined by the representation enclosed in the request message payload


예를 들어, PUT 요청 시 요청을 일부분만 보낸 경우 나머지는 default 값으로 수정되는 게 원칙이므로, 바뀌지 않는 속성도 모두 보내야 한다. 

(만약 전체가 아닌 일부만 전달할 경우, 전달한 필드외 모두 null 혹은 default 값처리되니 주의해야한다.)  
  
예를 들어, PUT HTTP 메소드로 gildong 이라는 유저의 나이(age)를 15로 변경하고자 할때 
수정된 값만 보낼 경우, 보내지 않은 데이터는 null로 변경되어 버린다.

```java
	PUT /users/1
	{
	    "age": 15 
	}
	
	HTTP/1.1 200 OK
	{
	    "name": null,
	    "age": 15
	}
```

따라서, PUT 요청 시에는 아래와 같이 변경되지 않는 데이터도 모두 전달해야한다. 

```java
	PUT /users/1
	{
	    "name": "gildong"
	    "age": 15
	}
	
	HTTP/1.1 200 OK
	{
	    "name": "gildong",
	    "age": 15
	}
```

그러나 PATCH를 이용하여  ‘age’만 변경하는 요청을 보내면, 

새롭게 바뀐 부분만 반영되며 나머지는 기존의 데이터가 유지된다.

```java
	PATCH /users/1
	{
		"age": 15
	}
	
	HTTP/1.1 200 OK
	{
		"name": "gildong",
		"age": 15
	}
```

따라서, 자원의 일부를 수정할 때는 PATCH를, 전체적인 수정이 필요할 때는 PUT을 이용하는 것이 적절하다.

출처: [https://devuna.tistory.com/77](https://devuna.tistory.com/77) [튜나 개발일기:티스토리]