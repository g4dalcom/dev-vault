# 개요

- 클라이언트와 서버가 어떠한 요청과 응답을 주고받을 때 그 사이사이에서 역할을 하는 기능들이 있습니다.
- 클라이언트가 요청을 보낼 수 있는 자격이 있는지(인증, 인가) 그리고 그 요청이 적절한 요청인지(유효성 검사) 등의 검사가 필요할 수도 있고, 요청과 응답에 무언가를 추가하거나 데이터를 제어해야 할 수도 있습니다.
- 이러한 요청과 응답의 전 과정을 **생명주기(life cycle)** 라고 하는데요! 이러한 라이프사이클에 관여하는 여러 기능들이 nest 공식문서에 소개되어 있습니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcXBkrM%2FbtsDnAKlZqC%2FFZdyqz5N3nE7Zg1BiFHAxk%2Fimg.png)

- 미들웨어, 필터, 파이프 가드, 인터셉터가 그 기능인데요. 공식문서를 보면 모두 Route Handler 에게 요청이 도달하기 전에(인터셉터는 전후로) 동작하는 무언가인 것 같은데 어떨 때 사용되는 것인지 명확하게 그림이 그려지지 않았습니다.
- 그래서 Nest.js 의 라이프사이클을 간단하게나마 이해하기 위해 정리해보았습니다.

# 라이프사이클(Life Cycle)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FDGTzU%2FbtsDnx7WcT6%2FNiE7Kzkn3LEdsjdtj6sw7k%2Fimg.png)

1. Incoming request
2. Globally bound middleware
3. Module bound middleware
4. Global guards
5. Controller guards
6. Route guards
7. Global interceptors (pre-controller)
8. Controller interceptors (pre-controller)
9. Route interceptors (pre-controller)
10. Global pipes
11. Controller pipes
12. Route pipes
13. Route parameter pipes
14. Controller (method handler)
15. Service (if exists)
16. Route interceptor (post-request)
17. Controller interceptor (post-request)
18. Global interceptor (post-request)
19. Exception filters (route, then controller, then global)
20. Server response

- 라이프 사이클을 나타내면 위와 같습니다. 
- 크게 보면, **middleware > guard > interceptor > pipe > interceptor > exception filter** 순으로 적용이 된다고 볼 수 있고
- 예외 필터를 제외하고는 글로벌 > 컨트롤러 > 라우트처럼 **범위가 큰 사이클부터 적용**이 됩니다.


## 미들웨어(Middleware)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdrT6dE%2FbtsDngejyea%2F9V17qMEhIcNG4kRhGfKPU0%2Fimg.png)

- 미들웨어는 라우트 핸들러 이전에 호출되어서 요청과 응답 객체에 접근하여 제어를 합니다.
- 요청 생명주기 맨 앞단에 위치하여 대체로 **로깅** 작업에 사용됩니다.
- Nest의 미들웨어는 기본적으로 express에서 구현하고 있는 미들웨어와 동일합니다.

```typescript
// logger.middleware.ts

import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    console.log('Request...');
    next();
  }
}
```

- @Injectable() 데코레이터가 있는 클래스가 **NestMiddleware** 인터페이스를 구현함으로써 기능을 정의할 수 있으며 next() 를 통해 다음 스택에 있는 미들웨어를 호출할 수 있습니다.

### 미들웨어 적용

#### 전역 범위 미들웨어

```typescript
// logger.middleware.ts

import { Request, Response, NextFunction } from 'express';

export const logger = (req: Request, res: Response, next: NextFunction) => {
  console.log('Request is received...');
  next();
};
```

```typescript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { logger } from './logger.middleware';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.use(logger);
  await app.listen(3000);
}
bootstrap();
```

- 위에서 작성된 미들웨어는 entry point에서 use() 함수를 통해 사용할 수 있습니다. 이 방식으로 정의된 미들웨어는 의존성 주입이 불가능하기 때문에 의존성이 필요한 전역 설정이 필요하다면 모듈 범위 미들웨어를 이용할 수 있습니다.

#### 모듈 범위 미들웨어

```typescript
import { Module, NestModule, MiddlewareConsumer, RequestMethod } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { LoggerMiddleware } from './logger.middleware';

@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(LoggerMiddleware)
      .forRoutes(
        {path: '/user', method: RequestMethod.GET,},
      );
      // forRoutes('*') -> 모든 경로, 모든 메소드 허용
  }
}
```

- 미들웨어를 포함하는 모듈은 **NestModule** 인터페이스를 구현해야 하며 **configure()** 메소드를 통해 설정합니다.
 - 모듈 범위 미들웨어들은 최상위 모듈에 바인딩된 미들웨어부터 작동하며, 
 - 그 이후에는 imports 배열에 추가되어있는 순서대로 각 모듈의 미들웨어들이 호출됩니다.


## 가드(Guard)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcAfrAa%2FbtsDqaYyKja%2FsLD9JJHSZaNuvuPwmgqnOK%2Fimg.png)

- 가드는 미들웨어와 마찬가지로 Route Handler 이전에 실행이 되지만 미들웨어와 다르게 **실행 컨텍스트에 접근이 가능**합니다.
- 미들웨어는 next() 함수 호출을 통해 다음 미들웨어를 호출하지만 어떤 미들웨어가 실행되는지는 알 수 없습니다. 하지만 실행 컨텍스트에 접근이 가능한 가드는 다음에 실행될 내용을 알 수 있기 때문에 코드가 선언적으로 유지되는 것에 도움이 됩니다.

```typescript
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { Observable } from 'rxjs';

@Injectable()
export class AuthGuard implements CanActivate {
  canActivate(
    context: ExecutionContext,
  ): boolean | Promise<boolean> | Observable<boolean> {
    const request = context.switchToHttp().getRequest();
    return validateRequest(request);
  }
}
```

- 가드는 **CanActivate** 를 구현하고 있는 @Injectable 클래스입니다.
- 주로 **인증과 인가**를 구현해서 로그인이 필요한 요청에 대해 로그인이 된 유저인지(인증) 확인하거나 권한이 있는 사용자의 요청(인가)인지를 확인할 수 있습니다.
- 위 코드의 경우는 validateRequest(request) 를 true로 통과한 요청만 다음 단계로 진행할 수 있도록 구현되었습니다.

```typescript
// 전역 가드 설정
const app = await NestFactory.create(AppModule);
app.useGlobalGuards(new AuthGuard());

// 종속성 주입이 필요한 경우
import { Module } from '@nestjs/common';
import { APP_GUARD } from '@nestjs/core';

@Module({
  providers: [
    {
      provide: APP_GUARD,
      useClass: AuthGuard,
    },
  ],
})
export class AppModule {}
```

```typescript
import { Controller, Get, UseGuards } from '@nestjs/common';
import { AuthGuard } from './auth.guard';

@Controller('app')
@UseGuards(AuthGuard) // 컨트롤러 레벨에서 가드 적용
export class AppController {

  @Get('protected')
  @UseGuards(AuthGuard) // 메서드 레벨에서 가드 적용
  protectedRoute() {
    return 'This route is protected!';
  }
  
}
```

- 정의된 가드는 전역으로 사용할 수도 있고 데코레이터와 데코레이터의 위치를 통해 더 좁은 범위에서 적용시킬 수도 있습니다.


## 파이프(Pipe)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcSlkdB%2FbtsDqo3jG8R%2Flxf7REL1zDXoZDPrFSzBmk%2Fimg.png)

- 파이프는 **PipeTransform** 인터페이스를 구현하고 있는 Injectable 클래스입니다.
- 두 가지 주요 기능은 아래와 같습니다.
	- transformation: 입력 데이터를 원하는 형식으로 변환합니다. ex) 문자열 -> 정수
	- validation: 데이터가 올바른 형식으로 입력되었는지 유효성 체크를 합니다.
- Nest는 메소드가 호출되기 직전에 파이프를 삽입하고 인수를 수신하여 제어합니다. 이 때 변환이나 유효성 검사와 같은 작업을 하고 제어된 인수를 넘겨서 최종적으로 메소드가 호출되게 됩니다.


### 파이프를 사용하는 방법

```typescript
// 전역 범위 설정
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(globalPipe)
  await app.listen(3000);
}
bootstrap();

// 핸들러 범위 설정
@UsePipes(pipe)
@Get(':id')
async findOne(@Param('id') id: number) {
  return this.catsService.findOne(id);
}

// 파라미터 범위 설정
@Get(':id')
async findOne(@Param('id', ParseIntPipe) id: number) {
  return this.catsService.findOne(id);
}
```

### 내장 파이프

1. ValidationPipe
2. ParseIntPipe
3. ParseFloatPipe
4. ParseBoolPipe
5. ParseArrayPipe
6. ParseUUIDPipe
7. ParseEnumPipe
8. DefaultValuePipe

- Nest에서 제공하고 있는 내장 파이프입니다.

```typescript
@Get(':id')
async findOne(@Param('id', ParseIntPipe) id: number) {
  return this.catsService.findOne(id);
}
```

- 만약 `GET localhost:3000/abc` 처럼 number 타입이 아니라 string 타입으로 요청을 보내면 아래와 같은 에러를 내보내게 됩니다.

```shell
{
  "statusCode": 400,
  "message": "Validation failed (numeric string is expected)",
  "error": "Bad Request"
}
```


## 인터셉터(Interceptor)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb7K114%2FbtsDqPl3bpV%2FlJ1yJbuC0hgYTxKGoZCY21%2Fimg.png)

- 인터셉터는 **NestInterceptor** 인터페이스를 구현하고 있는 Injectable 클래스입니다.
