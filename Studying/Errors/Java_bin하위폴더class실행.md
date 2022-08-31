---
keyword : Java, Path, Setting
class : ERROR
---


### bin 하위폴더 class 실행 못하는 문제

> 
> Error: Could not find or load main class 클래스이름 Caused by: java.lang.ClassNotFoundException: 클래스이름
> 

- cmd 창에서 “java 클래스이름” 명령어를 이용해 클래스파일을 실행하려고 할 때, bin폴더 내에 있는 class들은 제대로 실행이 되지만 bin폴더에서 하위폴더를 만들어서 실행을 하면 위와 같은 오류가 생기면서 실행이 되지 않았다.


#### 원인

- CLASS_PATH의 값이 오로지 bin폴더만 찾아보게 설정되어 있어서라고 예상함


---

#### 해결

![캡처.JPG](C:\Users\User\iCloudDrive\iCloud~md~obsidian\g4dalcom\img\error.jpg)

- CLASS_PATH의 변수값을 기존 %JAVA_HOME%\lib 에서 ;.;을 추가한 후에 cmd창에서 bin폴더로 이동한 후 “패키지이름.클래스이름”을 입력함으로써 실행할 수 있었다.


---

#### 연결문서

- [[ErrorsView]]