---
keyword : Spring, Server, H2, Port
class : ERROR
---


### Spring_ServerAlreadyInUse

> 
>Description:
>
>Web server failed to start. Port 8080 was already in use.
>
>Action:
>dentify and stop the process that's listening on port 8080 or configure this application to listen on another port.
>
>Process finished with exit code 1
> 


---

#### 원인

- 포트가 이미 실행 중일 때 스프링 서버를 Run 하면 실행되는 에러이다.


---

#### 해결

- 명령 프롬포트(CMD)에서 **netstat -ano**를 실행
- 
![img](C:\Users\User\iCloudDrive\iCloud~md~obsidian\g4dalcom\img\servererror.png)
- 8080 port의 TID를 찾아서 종료시키면 된다.
- 명령어 :  taskkill /pid xxxxx /f


---

#### 연결문서

- [[ErrorsView]]