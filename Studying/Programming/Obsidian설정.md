# 소개

- 신입 개발자는 자신이 꾸준히 공부한다는 것을 증명하기 위해 깃허브 잔디도 심어야 하고 블로그도 해야 한다고 한다.. 💢 프로그래밍 배우기도 바쁜데 여간 고단한 게 아닐 수 없다.
- 과정이 힘들면 금방 지치게 된다. 메모 어플에 기록하는 것만으로 깃허브와 블로그까지 간편하게 관리해서 과정을 심플하게, 꾸준한 습관으로 발전시켜보고자 한다! 
- 옵시디언은 마크다운을 기반으로 한 에디터이다. 마크다운은 약간의 진입장벽이 있지만 마크다운이기 때문에 깃허브와 블로그를 한 번에 관리할 수가 있다.

### 장점

- 로컬 폴더 위에서 동작하기 때문에 속도가 빠르고 외부 영향을 받지 않는다.
- 각종 유용한 플러그인들을 입맛에 맞게 설치해서 커스터마이징할 수 있고 프로그래밍 실력만 된다면 기능이나 디자인을 무한정 확장 가능하다.
- git으로 관리가 가능하고 깃허브와 연동시켜서 관리하기 매우 편리하다!


### 단점

- 약간의 러닝커브가 있다. 마크다운은 필수적으로 학습해야 하고 사용하려는 기능에 따라 git이나 쿼리 등의 부가적인 학습도 필요할 수 있다. 
	- 그러나 개인적으로 마크다운은 몇 시간이면 익숙해지고 그 외 기능들은 정말 필요한 경우에만 간단하게 학습하면 되기 때문에 마크다운에 대한 거부감만 없다면 진입을 츄라이하길 추천!
- PC-모바일 연동을 위해서는 유료 결제가 필요함. iCloud나 구글 드라이브 등을 이용해서 억지로 연동하는 방법도 있긴 하다.
- 마크다운 기반이라 표 그리기가 매우 불편하다.
- 한글로 된 레퍼런스가 적다.


# 시작하기

- [설치부터 기본사용법까지](https://www.babang9.com/entry/2-%EC%98%B5%EC%8B%9C%EB%94%94%EC%95%88-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0)
- [마크다운 사용법](https://goddaehee.tistory.com/307)


# 플러그인

### 플러그인 추가 방법
- 설정 -> 커뮤니티 플러그인 -> 탐색에서 설치하고자 하는 플러그인을 검색하고 설치하면 된다!

### 추천하는 플러그인!

- 표 그리기 간편한 방법을 제공
	- advanced tables + table generator
- 캘린더 및 데일리, 위클리 등 다이어리 쓰기
	- calander + periodic Notes
- 깃 연동하기
	- obsidian git
- 그 외 플러그인 탐색탭에 들어가면 인기순으로 플러그인들이 추천되기도 하고, 노션의 칸반보드와 같은 기능을 제공하는 kanban, 코드의 오류를 잡아주는 Editor syntax highlight 등 무궁무진한 플러그인이 존재한다.
- 사용하다가 뭔가 불편한데? 무슨 기능이 있었으면 좋겠는데? 하는 게 있다면 웬만하면 플러그인으로 나와있을테니 검색해서 사용해보자!

# Obsidian으로 깃허브와 블로그 한 번에 관리해보자!

#### 1. github desktop app 설치
- 설치 전에 옵시디언 vault는 생성해두어야 한다!

#### 2. 레포지토리 생성하고 연동하기
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fy51UI%2FbtrY7L5phUW%2FaDcmr20LigugvOI0XAsNv0%2Fimg.png)
- "하드드라이브에서 기존 레포지토리 추가"를 누른다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbG0WWv%2FbtrZchaYBgJ%2FTWOBkX42qzLgN7vSTKBjFK%2Fimg.png)
- 연동하고자 하는 옵시디언 볼트의 폴더를 선택해준다. 이 때 "디렉토리가 git 리포지토리가 아닌데 저장소를 만들겠습니까?" 라는 메시지가 뜨는데 create a repository 를 눌러준다!

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FRcmKm%2FbtrZhRP1sG8%2Foqg8OKvzCTNUk2mNKpk8xK%2Fimg.png)
- Local Path의 폴더이름과 Name을 일치시켜서 만들어준다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbjDJLY%2FbtrZdfDVAao%2FoZDVFtSxMZquEAKVEaR451%2Fimg.png)
- publish하면 폴더가 깃허브에 게시된다!

#### 3. Obsidian git 설치

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fd5l0I1%2FbtrZbRDBGqT%2Fk7RNwHm4osFsvSRmXYL47k%2Fimg.png)
- 그냥 git 검색해서 설치해버리기


![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb0SDPh%2FbtrY7Ldj1SD%2FbvrWedkMmU92XinSAaggkk%2Fimg.png)
- 설치 후 설정창에 들어가면 여러가지 설정이 있는데 중요한 것은 두 번째 `Vault backup interval` 이다. 얼마 간격으로 자동으로 백업할 것인지 설정하는 곳인데, 권장하는 것은 성능을 위해서 5~10분 간격으로 설정하라고 되어있지만 나는 자동백업을 하지 않고 내가 직접 하고 싶을 때에 푸쉬하고 있다.

#### 4. 깃허브 readme 생성하기

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FBrfEB%2FbtrZewk9lEl%2FnK6TXtp5K1tjRLAJlSLBY1%2Fimg.png)

- 깃허브와 내 obsidian vault가 연결은 되었는데 일일이 폴더에 들어가서 문서를 직접 열어봐야 하는 것이 번거롭다.
- readme 파일을 만들어서 위와 같이 폴더를 링크해주자!

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FX7mjb%2FbtrZg8j2bFi%2FPMjF7stGoAXRcMkCHR3V3K%2Fimg.png)
- 깃허브 레포지토리에 반영이 된 모습!

#### 5. .gitignore 설정

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdLSf3A%2FbtrZa3LikHs%2F6j6gJvb8tLtdkmAWuVWq90%2Fimg.png)
- 만약 내 obsidian vault 폴더 중에 깃허브에 올리고 싶지 않은 폴더가 있다면 vault 폴더에서 .gitinore.txt를 만든 후 ignore 할 폴더명을 위와 같이 써주면 된다!

#### 6. 깃허브에 게시하기

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fw4W1I%2FbtrZbPFKGDM%2Fn44WBRAvJbNPgk4Y9LlTs1%2Fimg.png)
- 수정된 내용이 있다면 github desktop을 실행하면 위처럼 tracking이 된다.
- 커밋해주고 푸시해주면 깃허브에 그대로 반영이 된다!

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcWbTIx%2FbtrZbo9vXbk%2FElPOBP0WmZVeTA9tOu7jkk%2Fimg.png)


#### 7. 티스토리에 게시하기

- 마크다운으로 게시가 가능한 모든 블로그에 적용이 가능하다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FQkqz6%2FbtrZddF7NIo%2FfSJ5hXo5rvpjQBVhou3DTk%2Fimg.png)
- 티스토리의 경우 글쓰기 모드 중 마크다운 모드가 따로 있다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FTkEbl%2FbtrZaihIi20%2Fm8JoVAUwxYxVSw0tYtfIiK%2Fimg.png)
- `마크다운 모드`에서 obsidian에 작성해둔 글을 그냥 복붙하면 마크다운 문법 그대로 들어간다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fo2i5h%2FbtrZa6nLmfD%2F4KxATKPMKbryQiQOEKdQB0%2Fimg.png)
- 모드를 다시 `기본모드`로 바꾸면 마크다운 문법들이 바뀌어서 보이게 된다.
- 그대로 게시하여도 무방하고 줄바꿈 등을 확인해서 수정한 후 게시하면 된다.