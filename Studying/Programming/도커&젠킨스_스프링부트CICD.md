Docker와 Jenkins를 이용한 스프링부트 CI/CD 연습을 해보았다!
기본적인 플로우는 우분투에서 도커를 띄우고 도커 내에 젠킨스와 스프링부트를 pull 받아서 스프링부트 프로젝트를 띄우는 것이다

###### 작업환경
-   Springboot 2.7.3
-   Java 11
-   Amazon EC2 t2.micro (프리티어)
-   ubuntu 18.04
-   Jenkins 2.376
-   Docker 20.10.21


### Jenkins Image 생성을 위한 Dockerfile, Shell Script 작성
- /home/ubuntu/example 이라는 폴더를 만들어서 넣어두었다.
```shell
#https://hub.docker.com/r/jenkins/jenkins
#pull jenkins image
FROM jenkins/jenkins:jdk11

USER root

#컨테이너 내에서 필요한 도커 설치
COPY docker_install.sh /docker_install.sh
RUN chmod +x /docker_install.sh
RUN /docker_install.sh

#설치 후 docker 그룹의 jenkins 계정 생성 후 해당 계정으로 변경
RUN groupadd -f docker
RUN usermod -aG docker jenkins
USER jenkins
```

### docker_install.sh 스크립트 작성
- 위에서 만든 Dockerfile과 같은 경로에 둔다.
```shell
#!/bin/sh
apt-get update && \
apt-get -y install apt-transport-https \
  ca-certificates \
  curl \
  gnupg2 \
  zip \
  unzip \
  software-properties-common && \
curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg > /tmp/dkey; apt-key add /tmp/dkey && \
add-apt-repository \
"deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
$(lsb_release -cs) \
stable" && \
apt-get update && \
apt-get -y install docker-ce
```

- [docker 설치를 위한 공식 shell script](https://github.com/docker/docker-install)

### 도커 설치
```shell
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
```
- Jenkins 이미지를 불러온 후 컨테이너 내부에서 Jenkins 의 command 에 포함될 docker 명령을 위해서 docker 를 설치한다.
- docker 를 설치할 때는 지속적으로 설치가 가능하도록 shell script 를 불러와서 실행한다.
- 여기서 한 가지 의문이 들 수 있다.
- docker 위에서 동작하는 컨테이너에서 다시 docker 를 설치한다?
- 컨테이너 내에서 docker 관련 명령을 수행하기 위해서(Jenkins 의 빌드 자동화 과정에서 docker build 명령어를 수행하는 등의) docker daemon 의 도움이 필요하다.

- 여기서 두 가지 선택이 가능한데
	1. Docker 위에 Docker Container 를 생성한다. (Docker In Docker, DInD)
	2. Local Machine 위에서 실행 중인 docker daemon 을 이용한다. (Docker out of Docker, DooD)

- 여기서 DInD 를 적용하기 위해서는 Docker Container 의 권한을 --privileged 로 설정해야하는데, 해당 권한이 Local Machine 의 대부분의 장치에 접근 가능하기 때문에 보안 상에 큰 단점이 존재하여 추천하지 않는 방식이라고 한다.,
- DooD 는 Local Machine 의 docker daemon 을 통해 docker 명령어를 수행하는 방식으로, 이 역시 100% 안전한 방식이라고는 할 수 없지만 권장되어 사용하는 방식이다.


### 빌드
```shell
docker build -t jenkins/test_jenkins:1.0 .
```

### 컨테이너로 올리기(run)
```shell
docker run -d -p 8080:8080 --name=jenkinscicd -v /home/ubuntu/example:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock jenkins/test_jenkins:1.0
```

### /var/run/docker.sok 권한 주기
`docker exec -it -u root 컨테ID /bin bash`

`chown root:docker /var/run/docker.sock`

`docker restart 컨테ID`

- 위의 DooD 를 적용하기 위해서 인데, Local Machine 의 Docker socket 을 Container 의 것과 공유하여, Local Machine Docker daemon 으로 명령어를 보낼 수 있게 한다.
- 하지만 여기까지 설정하고, 실제로 Jenkins 를 사용하다보면 docker 명령어에 대한 permission denied 가 뜰 수 있다.

- 현재까지의 상태를 나열해 보면
	container 에서 docker 를 설치한 유저 : root
	docker.sock 의 접근 권한 : root group level
	현재 container 의 유저 : docker(usermod 명령어 참고)

- 따라서 container 의 권한을 변경할 수는 없으니 container 내부의 docker.sock 파일의 권한을 변경해주도록 한다.
- Docker Container 는 일종의 프로세스로 여겨지면서도 가장 큰 차이점은 Container 만의 독립된 파일 시스템이 존재한다는 것인데
- 이를 통해 터미널로 Container에 접속하여 파일 수정이 가능하다는 것을 의미한다.

### Jenkins 초기세팅
- 비밀번호 저장을 위해 로그 보기
`docker logs 컨테ID`

- EC2 IP:8080 접속 후 초기 세팅 해주기

### Github Webhook을 이용한 CI/CD

###### 시스템 아키텍쳐
1. 브랜치에 push 를 한다.
2. GitHub Webhook 을 Jenkins 에게 전달한다.
3. Webhook 을 받은 jenkins project 는 빌드를 진행한다.
4. 빌드과정 (Jenkins 가 하는 일)
     - 타겟 repository 를 pull 한다.
     - 현재 위치(repo root) 에서 gradle build 를 수행한다.
     - docker build 를 수행한다. (spring boot image 생성)
     - docker run spring-boot-image 를 수행한다

- 같은 호스트 머신에서 젠킨스와 스프링 부트 서버 컨테이너를 동시에 띄운 이유는 아래와 같다
1. 서로 다른 머신에 Jenkins, SpringBoot 를 띄우면 추가적으로 ssh 연결을 통해서 배포가 트리거 되어야하는데, 당장의 규모가 작은 프로젝트에서 해당 환경을 셋팅하는데 들어가는 시간이 너무 길 것 같아서 최대한 간략한 CI/CD 환경을 세팅했다.

### 프로젝트 최상단에 Dockerfile 생성
```shell
#SpringBoot 구동에 필요한 jdk11 
FROM openjdk:11 

#변수 생성(상대 경로로 작성)
ARG JAR_FILE=build/libs/프로젝트이름-0.0.1-SNAPSHOT.jar

#(추가할 파일 : 이름) -> Docker 컨테이너 내부에 생성된다. 
COPY ${JAR_FILE} app.jar 

#(image 의 container 에서 필요한 저장소 경로)
VOLUME /tmp

#(도커 컨테이너 내부에서 몇번 포트로 돌 것인가)
EXPOSE 8081

#(실행할 명령어, 컨테이너 내부에 생성될 경로로 생각)
ENTRYPOINT ["java","-jar","/app.jar"]
```
- `git add .` `git commit -m "message"` `git push` 해서 레포지토리에 Dockerfile 추가해준다


### Jenkins Item 설정

- 새로운 Item 추가 버튼으로 Freestyle project 를 생성한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2Fs4Mly%2FbtrQr2mw3Ak%2F4gJkLh0kUFzUgH8A4xL9B0%2Fimg.png)
- Github 주소 입력 

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FbNWbtN%2FbtrQq5jVP0F%2FbQZKKo68aeKWkNcbiRkDG1%2Fimg.png)
- 클론할 레포지토리 입력하고 브랜치 이름 입력(기본 master)
- Private repo 의 경우에는 AccessToken 이 있어야만 접근 가능하기 때문에 위와 같은 방법 혹은Credentials 을 별도로 생성해서 git Username-Access Token 을 기입하도록 한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FcG5YF3%2FbtrQrHv4Wq5%2FPUiDyhrkuox5sP3054qnf1%2Fimg.png) 
- 위에서 명시한 GitHub 프로젝트 경로와 일치하는 경우에  GiHub Webhook 을 받는다는 것을 의미하고, 사전에 젠킨스에 설치한 GitHub Plugin 은 프로젝트의 변경사항을 감지하고, 빌드를 수행한다는 것을 의미한다.

 
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FcMp4Jd%2FbtrQrrmHX0v%2FctcW1KrhY8kXi8RIRbNZdk%2Fimg.png)
- Execute shell 을 통해 직접 커맨드를 추가한다.
- 각각의 커맨드는 따로따로 Execute shell 로 추가해도 되고, \ 를 이용해서 한꺼번에 수행해도 되지만
- 개인적으로 각각 추가했을 때 Jenkins Console 에서 보기 편해서 각각 추가해줬다

```shell

!/bin/sh

# SpringBoot 프로젝트를 jar 파일로 빌드한다.
./gradlew clean build

# jar 파일을 포함하여 SpringBoot image 를 생성한다.
docker build -t username/spring-boot:1.0 .

# 기존에 동작중인 container 의 id 를 찾고, 존재하면 container 를 내린다.
docker ps -q --filter "name=spring-boot-server" | grep -q . && docker stop spring-boot-server && docker rm spring-boot-server | true

# 위에서 생성한 SpringBoot 이미지를 구동시킨다.
docker run -p 8081:8080 -d --name=spring-boot-server username/spring-boot:1.0

# 태그가 겹친 이미지는 tag 가 none 이 되므로 해당 이미지들을 삭제한다.
docker rmi -f $(docker images -f "dangling=true" -q) || true
``` 

### 수동으로 빌드해보기
- 우여곡절을 겪고 성공했다
- 쉘 스크립트 오타, Docker 용량부족 등 자잘한 이슈로 인해서 29번만에 성공한 모습이다^^
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FbFA9Oy%2FbtrQr6PUdCs%2FpkF9t2ZzpZk1aifHxKdok0%2Fimg.png)

- console output으로도 확인!
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FquKQz%2FbtrQsEyUf1r%2FpMpJbhJeHBQ6VrA1fUAXZ1%2Fimg.png)


- 8081번 포트로 들어가면 프로젝트가 정상동작한다!
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FcjgIgN%2FbtrQq5jV681%2FjKYlkiDwUQKFAP3SyiWeo1%2Fimg.png)


### Github Webhook 설정

- Github Repository -> Settings -> Webhooks에서 설정한다
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FbkAOGl%2FbtrQqI3DRFr%2FWsDPggbmf8bm4rBBKAyXxk%2Fimg.png)
- Payload URL = http://ec2IP:젠킨스port/github-webhook/
- Content type = application/json
- event 설정은 push할 때만으로 우선 설정해두었다.

- 프로젝트를 수정한 후 push를 하면 자동으로 업데이트가 된다!

##### private repository일 경우 참고

- CI/CD 자동화를 위해서는 젠킨스의 ssh 키 값이 필요하다. 아래 명령어를 통해 id_rsa.pub와 id_rsa의 내용을 복사해두자. 참고로 id_rsa는 --- 부분도 모두 복사해야한다.

`docker exec -it 컨테이너ID ssh-keygen`
`docker exec -it 컨테이너ID cat /var/jenkins_home/.ssh/id_rsa.pub`
`docker exec -it 컨테이너ID cat /var/jenkins_home/.ssh/id_rsa`

- Github webhook을 이용할 깃허브 프로젝트에 들어가서 Setting -> Webhooks -> Add webhook을 클릭한다.

```plaintext
Payload URL : http://젠킨스ip:젠킨스port/github-webhook/
Content type : application/json
Setting -> Deploy keys -> Add deploy key 클릭

Title : 아무거나
Key : 5번에서 복사한 id_rsa.pub 값
젠킨스에 접속 후 Jenkins 관리 -> 시스템 설정 -> Add GitHub Server 클릭 후 Credentials ADD를 해준다.

Kind : Secret text
Secret : 2번에서 복사한 access token 키 값
ID : 깃허브 ID
```


---

### 참고자료

[참고사이트1](https://ppaksang.tistory.com/6?category=1035114)
[참고사이트2](https://prod.velog.io/@y00913/%EB%8F%84%EC%BB%A4-%EC%A0%A0%ED%82%A8%EC%8A%A4%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-CICD)