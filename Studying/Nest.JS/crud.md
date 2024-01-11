# 개요

- postgreSQL 데이터베이스에 실제로 데이터를 저장하고 꺼내오는 등 데이터 조작을 해볼 예정입니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbqAI6v%2FbtsDnzXzxuD%2Fz9yuzfzfKF7VKozohXGMP1%2Fimg.png)

- 요청이 들어오면 컨트롤러가 받아서 서비스에게 넘겨주고, 서비스는 해당 로직을 처리합니다. 이 때 데이터가 필요하다면 레포지토리를 통해 불러올 것이고 데이터를 저장해야 한다면 이 또한 레포지토리를 통할 것입니다.
- 레포지토리는 데이터를 알맞는 테이블과 매핑하여 데이터베이스를 조작합니다.
- 각 단계마다 데이터들은 DTO 객체로 감싸서 통신이 될 것이고 typeORM이 DTO 객체를 클래스 객체로 변환하여 데이터베이스와 통신합니다. 반대로 데이터베이스에서 받아온 데이터를 DTO로 변환해서 응답합니다.

# DTO 생성

- DTO는 Data Transfer Object 의 약자입니다. 말 그대로 데이터를 담고 이동하는 객체입니다.
- 클래스 객체를 그대로 이용하는 것은 변조의 위험이나 데이터의 무결성을 헤칠 수 있고 또한 클래스에서 정의한 속성들 중 필요한 부분만을 사용할 수 있다는 점에 의의가 있습니다. 또한 클래스 속성이 변동되었을 때도 DTO는 영향을 덜 받을 수 있겠죠!
- 이러한 이유로 단계별 데이터는 DTO로 감쌀 것이고 최종적으로 레포지토리와 통신할 때 적절한 클래스와 매핑할 것입니다.

```typescript
// user/dto/request.dto.ts

export class CreateUserDto {
  username: string;
  age: number;
}
```

- 우선 유저 생성에 필요한 DTO를 만들어볼까요?
- DTO를 만들 때 고민해야 하는 점은 실제로 요청을 받을 때 입력받아야 하는 것이 무엇일까 하는 것입니다.
- User 클래스에는 속성으로 id, username, age, role 이 있는데 이 중 id는 데이터베이스에서 자동으로 부여되는 것이고 role 또한 default 값이 있으므로 따로 입력받을 필요가 없습니다. 그렇기 때문에 DTO에는 username과 age만 입력받아도 되는 것이죠!
- 참고로 DTO는 class와 interface로 정의할 수 있는데 user는 인스턴스 객체를 만들 수 있어야 하기 때문에 class로 정의하였습니다.

# Create

