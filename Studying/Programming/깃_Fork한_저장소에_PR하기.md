---
keyword : Git
class : Programming
---

# Fork한 저장소에 Pull Request 하기

클론코딩 프로젝트를 진행 중에 fork한 팀원의 원본 저장소에 PR을 하자, "This branch has conflicts that must be resolved" 라는 메시지와 함께 PR이 되지 않았다.

원인은 다른 팀원이 PR한 것이 원본 저장소와 merge가 되었고 그 내역을 내가 가지고 있지 않기 때문에 먼저 merge된 원본 저장소 내역을 pull해야 했다.

순서는 아래와 같다!

- 현재 원격 저장소가 무엇이 있는지 확인
```
$ git remote -v
```

- 원본 저장소를 원격 저장소로 추가(예시는 upstream이라는 이름으로 저장함)
```
$ git remote add upstream https://github.com/originalOwner/original_repository
```

- 원격 저장소에 추가된 것 확인
```
$ git remote -v
```

- 추가한 upsteram 저장소를 fetch
	- fetch와 pull은 가져와서 머지까지 자동으로 해주는지의 차이이며 fetch는 머지까지 자동으로 되지 않는다.
```
$ git fetch upstream
```

- 자신의 master로 checkout
```
$ git checkout master
```

- fetch 해두었던 upstream/master를 체크아웃한 master 브랜치로 머지
```
$ git merge upstream/master
```

- 충돌 해결한 후 push하여 내 저장소를 최신화하고 PR을 보내면 된다!