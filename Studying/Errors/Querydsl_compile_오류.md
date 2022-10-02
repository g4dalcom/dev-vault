---
keyword : Spring
class : ERROR
---


### Querydsl compile 오류


#### 에러내용

- compileQuerydsl 시 에러
- 에러메시지
	- Unable to load class 'com.mysema.codegen.model.Type'.
	- This is an unexpected error. Please file a bug containing the idea.log file.


---

#### 해결

- dependency에 추가

```java
/* querydsl 추가 */
implementation 'com.querydsl:querydsl-jpa'  
annotationProcessor "com.querydsl:querydslapt:${dependencyManagement.importedProperties['querydsl.version']}:jpa"  
annotationProcessor "jakarta.annotation:jakarta.annotation-api"  
annotationProcessor "jakarta.persistence:jakarta.persistence-api"
```

---

