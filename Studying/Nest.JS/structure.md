# 개발환경

- Windows 10
- node.js v21.5.0
- nest.js 10.3.0

## 환경 세팅

### Windows에서 node.js 최신 버전 업데이트 하기

- [node 공식 사이트](https://nodejs.org/en) 에서 직접 다운로드 후에 설치를 진행해도 되고 
- nvm 을 이용해서 설치해도 됩니다. nvm은 여러 노드 버전들을 손쉽게 관리할 수 있기 때문에 저는 nvm을 설치해서 업데이트 하였습니다.

#### nvm 설치하기

- [nvm github](https://github.com/coreybutler/nvm-windows/releases) 에서 nvm-setup.exe 파일을 다운로드 후에 설치를 하면 됩니다!
- 주요 nvm 명령어는 아래와 같고 저는 `nvm install node`를 이용해서 최신버전을 설치하였습니다. 만약 프로젝트 규모가 클 것으로 예상이 된다면 LTS 버전을 확인해서 설치하는 게 좀 더 안정적일 수 있을 것 같네요!

```shell
nvm version             // nvm version 확인
nvm install node        // node 최신버전 설치
nvm install <version>   // node 특정버전 설치
nvm list                // 사용가능한 node 확인 
nvm use <version>       // 해당 node 버전 사용
```

#### npm 최신버전 업데이트

```shell
npm install -g npm@latest
```

- nest.js 의 경우 npm 버전이 낮으면 설치가 되지 않을 수 있으니 npm도 업데이트를 해줍시다!

#### Nest.js 설치

- nest.js 는 공식문서가 잘 되어있기 때문에 그대로 따라가기만 해도 됩니다.

```shell
npm install -g @nestjs/cli
nest new <PROJECT_NAME>
```

- nestjs cli를 설치한 후 new 명령어로 프로젝트를 생성합니다.
- 만약 현재 폴더에 그대로 프로젝트를 설치하고 싶다면 <PROJECT_NAME> 에 `./`를 입력하면 됩니다!

## 기본 구조 살펴보기

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdGou3c%2FbtsDayMs4eW%2F0CcuvDT8UKQvmSjroo6O30%2Fimg.png)

- nest 프로젝트를 생성하면 설정 파일들을 제외하고 src 폴더에 기본 구조가 생성이 됩니다.

```typescript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
```

- listen() 메소드의 3000은 3000번 포트를 기본으로 열겠다는 의미로 보이는데요!
- `npm start dev` 명령어로 서버를 실행시킨 후에 `http://localhost:3000` 으로 접속해보면 Hello World 라는 문구가 뜨는 것을 확인할 수 있습니다.
- 그러면 이 Hello World 가 어디에서 온 것인지 살펴볼까요?!
- 우선 NestFactory의 create() 메소드에 인자로 들어가있는 AppModule을 살펴봐야 할 것 같습니다.

### app.module.ts

```typescript
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

- app.module은 가장 최초 진입점(root)의 컨트롤러와 서비스를 연결하고 있는 최상위 모듈로 보입니다.
- Nest.js 에서의 모듈은 기능별로 구분된 집합 요소입니다. 가령 User 관련한 기능들(UserController, UserService, UserEntity)은 User라는 하나의 모듈(user.module) 안에서 관리되는 것이죠.
- 따라서 하나의 프로그램에는 기본적으로 app.module 이라는 루트 모듈이 존재하고 이외에도 여러 모듈들이 존재할 수 있는 것입니다!
- 스프링에서 제어의 역전 개념이 구현된 스프링 빈과 nest의 모듈이 비슷한 개념이라고 느껴졌습니다. 스프링이 컴포넌트 스캔을 통해 스프링 빈을 파악하고 관리하듯이 nest는 정의된 모듈들과 요소들을 매핑하고 싱글톤으로 관리해주는 것 같네요!

### app.controller.ts

```typescript
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return this.appService.getHello();
  }
}
```

- 자바의 어노테이션과 같이 해당 클래스 상단에 @Controller() 가 붙어있는데 이는 **데코레이터 함수** 라고 합니다. 타입스크립트에서 제공하는 기능이라고 하네요!
- 그리고 익숙한 @Get() 데코레이터도 보이는데 스프링부트와 생김새가 매우 비슷해서 코드를 보자마자 역할을 파악하기 쉬웠습니다.
- **GET** 메소드이므로 **/** 경로로 요청이 들어왔을 때 실행이 되는 메소드라고 생각이 되네요! 이 메소드는 요청이 들어오면 appService의 getHello() 메소드를 실행시키는군요!

### app.service.ts

```typescript
import { Injectable } from '@nestjs/common';

@Injectable()
export class AppService {
  getHello(): string {
    return 'Hello World!';
  }
}
```

- 3000번 포트로 진입을 했을 때 나타났던 Hello World라는 문구가 바로 이곳에서 리턴이 됩니다.
- 요약하면, **/** 경로로 get 요청이 들어오고 해당 요청을 처리할 AppController 가 AppService의 처리된 로직을 받아서 최종 반환한 것입니다!
- 여기서 스프링부트와 다른 점은 @Service 어노테이션이 아니라 @Injectable() 이라는 데코레이터가 붙어있다는 점인데요! Injection 이라고 하면 스프링을 공부할 때 귀에 피가 나도록 듣는 DI(Dependency Injection)이 생각이 납니다. DI라는 것은 의존성을 주입한다는 것인데요 **injectable을 해석하면 주입가능한** 이므로 해당 모듈을 주입가능한 상태로 만들어주는 데코레이터라고 추측할 수 있습니다.
- @Injectable() 데코레이터에 대해서는 공식문서에 관련 설명이 있습니다.

> 1. In cats.service.ts, the @Injectable() decorator declares the CatsService class as a class that can be managed by the Nest IoC container.
> 2. In cats.controller.ts, CatsController declares a dependency on the CatsService token with constructor injection:  
>     `constructor(private catsService: CatsService)`
> 3. In app.module.ts, we associate the token CatsService with the class CatsService from the cats.service.ts file. We'll see below exactly how this association (also called registration) occurs.

- 간단하게 정리해보자면,
- 주입할 프로바이더에 `@Injectable` 데코레이터를 붙이면 Nest IoC 컨테이너가 해당 프로바이더를 관리할 수 있다고 선언합니다.
- 주입 받을 곳에는 주입 받을 프로바이더의 토큰을 생성자 주입 방식으로 선언합니다.
- 모듈에 둘을 등록하면, 둘을 연결시켜줍니다.
- 마치 스프링 IoC 컨테이너가 Spring Bean을 관리하는 것과 유사한 것 같습니다.

## 모듈 생성해보기

- nest.js는 모듈을 생성하는 간편한 CLI를 제공합니다.

```shell
nest g module <MODULE_NAME>
```

- user 와 관련된 기능을 만들고자 하면 user 모듈을 우선 만들어야겠죠?! `nest g(generate) module user` 명령어로 모듈을 생성해보겠습니다!

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbJkijj%2FbtsDjXX0EC7%2FN9X2AMIZq8Q7W3l7UF5g00%2Fimg.png)

- 그러면 src 폴더 아래에 module_name으로 설정한 폴더가 생성이 되고 그 안에 모듈이 생성됩니다. 이전에 보았던 app.module.ts 와 구조가 같습니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fn5eIe%2FbtsDfO83cjs%2FD5p5kldLSyDEJOO7UbbkLk%2Fimg.png)

- 그리고 app.module.ts 에 자동으로 UserModule이 import가 되는 것도 확인할 수 있습니다.
- 요청이 들어오면 import 된 모듈들을 스캔하면서 해당 요청을 처리할 모듈에게 요청을 넘기는 Front Controller 와 같은 역할을 할 수 있을 것 같네요!

## 모듈 내 요소 생성

- 이어서 user 모듈 내의 요소들을 생성해보겠습니다. 이 또한 CLI 가 존재합니다.

```shell
nest g controller user --no-spec
nest g service user --no-spec
```

- 여기서 **--no-spec**은 기본적으로 생성되는 test 파일을 생성하지 않도록 하는 명령어입니다.
- 기본 nest 파일 구조에는 app.controller.spec.ts 이라는 파일이 존재하는데 이러한 테스트 파일을 만들지 않겠다는 뜻이죠!
- 기본 구조를 파악하기 위한 연습이므로 테스트 파일은 생성하지 않았습니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FF0BLR%2FbtsDhiovxhp%2Fo0SbCn5IOB3vAMif6vgayK%2Fimg.png)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FsKybw%2FbtsDfpOZ6pY%2F7AoOlrFeER3hKXQFszEo0K%2Fimg.png)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FrjWGD%2FbtsDiRX1kuv%2FpxKPcTu62HUCLu1y3qJOmk%2Fimg.png)

- 모듈들을 생성하면 위와 같이 기본 구조가 폴더 내에 생성이 되고 user.module 에도 연결이 됩니다.
- Controller의 진입점이 'user' 로 기본 설정되어 있는 부분도 눈여겨볼 부분이겠네요!

## 기능 구현

##### user.service.ts

```typescript
import { Injectable } from '@nestjs/common';

@Injectable()
export class UserService {
  getAllUsers(): string {
    return '옛다! users info';
  }
}
```

##### user.controller.ts

```typescript
import { Controller, Get } from '@nestjs/common';
import { UserService } from './user.service';

@Controller('user')
export class UserController {
  constructor(private userService: UserService) {}

  @Get()
  getAllUsers(): string {
    return this.userService.getAllUsers();
  }
}
```

- user entity 를 생성하기 전에 컨트롤러와 서비스가 제대로 동작하는지 알아보기 위해 간단한 로직을 작성해보았습니다.
- 우선 service 로직을 사용하기 위해 의존성을 주입해야 합니다. 스프링부트와 마찬가지로 생성자 주입이 가능합니다!
- 참고로 @Controller 데코레이터에 정의되어 있는 'user'는 스프링에서 @RequestMapping('/user') 과 같은 역할을 합니다. 반복적으로 사용되는 경로를 접두사로 지정해두는 것이죠!
- 이제 **/user** 라는 엔드포인트로 들어오면 get 요청을 날리게 되고 userService의 getAllUsers() 메소드를 실행시키며 getAllUsers() 는 단순하게 "옛다! users info" 를 반환합니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F1WXWX%2FbtsC8YdTv6b%2FaneasOvEU0SXkYYjxVsLKk%2Fimg.png)

- http://localhost:3000/user 로 접속하면 제대로 동작함을 확인할 수 있습니다!