# 개요

- postgreSQL 데이터베이스에 실제로 데이터를 저장하고 꺼내오는 등 데이터 조작을 해볼 예정입니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbqAI6v%2FbtsDnzXzxuD%2Fz9yuzfzfKF7VKozohXGMP1%2Fimg.png)

- 요청이 들어오면 컨트롤러가 받아서 서비스에게 넘겨주고, 서비스는 해당 로직을 처리합니다. 이 때 데이터가 필요하다면 레포지토리를 통해 불러올 것이고 데이터를 저장해야 한다면 이 또한 레포지토리를 통할 것입니다.
- 레포지토리는 데이터를 알맞는 테이블과 매핑하여 데이터베이스를 조작합니다.
- 각 단계마다 데이터들은 DTO 객체로 감싸서 통신이 될 것이고 typeORM이 DTO 객체를 클래스 객체로 변환하여 데이터베이스와 통신합니다. 반대로 데이터베이스에서 받아온 데이터를 DTO로 변환해서 응답합니다.

# DTO 생성

- DTO는 Data Transfer Object 의 약자입니다. 말 그대로 데이터를 담고 이동하는 객체입니다.
- 클래스 객체를 그대로 이용하는 것은 **변조의 위험이나 데이터의 무결성을 헤칠 수 있고** 또한 **클래스에서 정의한 속성들 중 필요한 부분만을 사용**할 수 있다는 점에 의의가 있습니다. 또한 **클래스 속성이 변동되었을 때도 DTO는 영향을 덜 받을 수 있겠죠**!
- 이러한 이유로 데이터는 DTO로 감쌀 것이고 최종적으로 레포지토리와 통신할 때 적절한 클래스와 매핑할 것입니다.

```typescript
// user/dto/request.dto.ts

export class CreateUserDto {
  username: string;
  age: number;
}
```

- 우선 유저 생성에 필요한 DTO를 만들어볼까요?
- DTO를 만들 때 고민해야 하는 점은 실제로 요청을 받을 때 입력받아야 하는 것이 무엇일까? 하는 것입니다.
- User 클래스에는 속성으로 id, username, age, role 이 있는데 이 중 id는 데이터베이스에서 자동으로 부여되는 것이고 role 또한 default 값이 있으므로 따로 입력받을 필요가 없습니다. 그렇기 때문에 request DTO에는 username과 age만 있어도 되는 것이죠!
- 참고로 DTO는 class와 interface로 정의할 수 있는데 user는 인스턴스 객체를 만들 수 있어야 하기 때문에 class로 정의하였습니다.

```typescript
// user/dto/response.dto.ts

import { UserRole } from '../user.entity';

export class ResponseDto {
  username: string;
  age: number;
  role: UserRole;
}
```

- 위와 같이 요청시 필요한 데이터와 응답시 필요한 데이터는 다를 수 있습니다.
- 이것들을 각각 DTO로 만들어서 필요한 데이터만 감싸서 보내줄 수 있겠죠?!

# Repository 의존성 주입

- Repository를 Service가 사용하려면 우선 repository를 주입받아야 합니다.

```typescript
// user.service.ts

@Injectable()
export class UserService {
  constructor(
    @InjectRepository(UserRepository)
    private userRepository: UserRepository,
  ) {}

  getAllUsers(): string {
    return '옛다! users info';
  }
}
```

- 다른 프로바이더와 달리 repository는 주입할 때 **@InjectRepository**라는 데코레이터와 함께 사용합니다.

# Create

```typescript
// user.controller.ts

import { Body, Controller, Post } from '@nestjs/common';
import { UserService } from './user.service';
import { CreateUserDto } from './dto/request.dto';
import { ResponseDto } from './dto/reponse.dto';

@Controller('user')
export class UserController {
  constructor(private userService: UserService) {}

  @Post()
  createUser(@Body() requestDto: CreateUserDto): Promise<ResponseDto> {
    return this.userService.createUser(requestDto);
  }
}
```

- 요청 body에 데이터를 보낼 것이므로 @Body() 데코레이터를 사용하고 CreateUserDto 타입의 dto 데이터를 받아옵니다.
- 이 데이터는 사용자에게 직접 입력받는 것이고 지금은 Postman을 이용해서 json 형식으로 보낼 것입니다.

```typescript
// user.service.ts

import { Body, Injectable } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { UserRepository } from './user.repository';
import { CreateUserDto } from './dto/request.dto';
import { ResponseDto } from './dto/reponse.dto';

@Injectable()
export class UserService {
  constructor(
    @InjectRepository(UserRepository)
    private userRepository: UserRepository,
  ) {}

  async createUser(@Body() requestDto: CreateUserDto): Promise<ResponseDto> {
    const user = this.userRepository.create(requestDto);
    await this.userRepository.save(user);

    const response = { ...user, role: UserRole.GHOST };

    return response;
  }
}
```

- 서비스에서는 controller에서 보내온 dto 객체를 이용해서 user 객체를 만든 후 repository를 통해 데이터베이스에 저장을 합니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FtCVqT%2FbtsDloiit56%2F4jW0IrWfGThN2YYtl9qMb0%2Fimg.png)

- postman으로 유저를 생성해보겠습니다.
- json으로 requestDto 형식에 맞게 body에 입력을 하면 responseDto 형식으로 응답이 오는 것을 볼 수 있습니다.
- 참고로 nestjs는 요청 성공시 https status를 POST가 201, 그 외는 200을 기본으로 하고 있습니다. 이것을 요청별로 제어하려면 @HttpCode() 데코레이터를 이용하면 됩니다.


![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fxxj3P%2FbtsDkQ0ixp7%2FiuubYgyOygIr29NKem9CWk%2Fimg.png)

- insert 쿼리가 잘 생성이 되었구요!

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbIhsjn%2FbtsDnvA6qg3%2FxkITktIPFvYDUFV3bPQnwk%2Fimg.png)

- 데이터베이스에도 잘 반영이 되었네요!


# Read

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FLlVQm%2FbtsDogQ3nfY%2FQWAcfLqs37frYMZxUmxqAk%2Fimg.png)

- 우선 데이터베이스에 유저를 몇 개 더 만들어두었습니다.

```typescript
// user.controller.ts

@Get('/all')
getUsers(): Promise<ResponseDto[]> {
return this.userService.getUsers();
}

@Get('/:id')
getUser(@Param('id') id: string): Promise<ResponseDto> {
return this.userService.getUser(Number(id));
}
```

- getUsers() 는 모든 유저의 목록을 불러오는 API입니다. 따라서 반환 타입도 ResponseDto[] 죠!
- getUser()는 파라미터로 id를 받아서 해당 id인 유저 하나만 받아옵니다.


```typescript
async getUsers(): Promise<ResponseDto[]> {
const user = await this.userRepository.find();

return user;
}

async getUser(id: number): Promise<ResponseDto> {
const user = await this.userRepository.findOneBy({ id: id });

if (!user) {
  throw new NotFoundException(`${id} 회원은 존재하지 않습니다.`);
}

return user;
}
```

- 서비스단도 크게 다를 게 없습니다. 다만 단일 유저를 불러오는 API는 user가 존재하지 않을 경우를 대비해서 예외처리도 해보았습니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FNohEG%2FbtsDkUuTiDy%2FFGrvVB73k6tUNMdYoU7k01%2Fimg.png)

- /user/all 로 요청을 보내면 모든 유저 목록을 볼 수 있고

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fce7zjd%2FbtsDmU8ZcSr%2FnohaOsK5vXyhRHBBkPJk40%2Fimg.png)

- 파라미터와 일치하는 유저도 잘 불러와지네요!

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FkU5wT%2FbtsDnDyVp0v%2FUON1mhUhkNKJd27TMn7Fyk%2Fimg.png)

- 존재하지 않는 4번 회원을 보내달라고 요청하면 작성한 예외처리 문구와 함께 에러 코드가 자동으로 오는 것도 편리한 점인 것 같습니다!

# Update

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FLlVQm%2FbtsDogQ3nfY%2FQWAcfLqs37frYMZxUmxqAk%2Fimg.png)

- 유저 리스트에서 1번 유저의 role을 admin으로 바꾸는 API를 구현해보겠습니다.

```typescript
// user.controller.ts

@Patch('/:id')
updateUserRole(
@Param('id') id: string,
@Body('role') role: UserRole,
): Promise<ResponseDto> {
return this.userService.updateUser(Number(id), role);
}
```

- HTTP 메소드는 PATCH 를 사용합니다. PUT과 PATCH 는 둘 다 update 작업을 위한 메소드인데 PUT은 해당 컬럼의 값을 전부 바꿀 때 사용하고 PATCH는 특정 값만을 바꿀 때 사용할 수 있습니다. role 한 부분만 수정할 것이므로 PATCH를 사용합니다.
- 그리고 어떤 유저의 정보를 바꿀 것인지 알아야 하므로 식별자인 id를 파라미터로 받을 것이고 바꿀 데이터는 body로 받을 것입니다.

```typescript
/// user.service.ts

async updateUser(id: number, role: UserRole): Promise<ResponseDto> {
const user = await this.userRepository.findOneBy({ id: id });
user.role = role;
await this.userRepository.save(user);

return user;
}
```

- 데이터베이스에서 유저 정보를 꺼내온 뒤 업데이트를 하고 다시 save를 하는 작업을 합니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcsFPXT%2FbtsDnvoTAq0%2FEumn4Tnc2FsC0TQwJjsosk%2Fimg.png)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcnfr3X%2FbtsDpvVWXV3%2Fk7b0dGgOdOddTD8ky7UkK1%2Fimg.png)

- postman으로 테스트를 해보면 데이터베이스에 정상적으로 반영이 된 것을 볼 수 있습니다!

# Delete

- 데이터베이스에서 데이터를 삭제하는 메소드에는 remove() 와 delete() 가 있습니다.
- `remove` - Removes a given entity or array of entities. It removes all given entities in a single transaction (in the case of entity, manager is not transactional). Returns the removed entity/entities.

```typescript
await repository.remove(user)
await repository.remove([category1, category2, category3])
```

- `delete` - Deletes entities by entity id, ids or given conditions:

```typescript
await repository.delete(1)
await repository.delete([1, 2, 3])
await repository.delete({ firstName: "Timber" })
```

- 공식문서에 따르면, remove()는 엔티티나 엔티티 배열을 인자로 받아서 해당 엔티티를 삭제합니다. 엔티티를 가지고 있을 때 사용할 수 있겠죠!
- delete()는 id 또는 특정 조건을 인자로 받아서 일치하는 엔티티를 삭제합니다.
- 저는 유저의 id를 받아서 해당 id인 유저를 삭제할 것이므로 delete() 메소드를 사용합니다.


```typescript
// user.controller.ts

@Delete('/:id')
deleteUser(@Param('id') id: string): Promise<void> {
return this.userService.deleteUser(Number(id));
}
```

```typescript
// user.service.ts

async deleteUser(id: number): Promise<void> {
await this.userRepository.delete(id);
}
```

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbkGcNi%2FbtsDqmRJcGU%2F06WxXFpfIFT1LXPyX9gsu1%2Fimg.png)

- 반환값을 void로 했기 때문에 응답으로 오는 것은 없지만 http status code 가 200인 것으로 보아 정상적으로 요청과 응답이 오고갔음을 알 수 있습니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F4VQDh%2FbtsDqlkY6n5%2FwTCGHSlCT3QGXLCdJ8ZpQ0%2Fimg.png)

- 데이터베이스에서도 해당 유저가 잘 삭제 되었네요!