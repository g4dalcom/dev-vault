# 데이터베이스 세팅

- 이번 연습 프로젝트에서는 비용에서 자유로운 RDBMS 중 하나인 postgreSQL을 사용해보기로 하였습니다.

## postgreSQL 설치

- [postgreSQL 다운로드 페이지](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fl7lo8%2FbtsDkAoPEqe%2F8sJjoR5LmxI4to56TArLA0%2Fimg.png)

- 다운로드 페이지에서 설치 파일을 다운로드 받은 뒤 진행하다보면 선택적으로 설치할 수 있는 파일 목록이 보이는데요!
	- postgreSQL server: postgreSQL을 사용하기 위한 프로그램
	- pgAdmin4: 데이터베이스 GUI
	- Stack Builder: 여러 추가 프로그램을 설치할 수 있는 도구
	- Command Line Tools: 명령어로 데이터베이스를 조작할 수 있는 도구
- 필요에 따라 선택적으로 설치를 하면 됩니다.


## 데이터베이스 생성

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbXElSw%2FbtsDkSCL8Xf%2FkONlD0Htjv0KniNQ8Gk4q0%2Fimg.png)

- Add New Server 를 눌러서 서버를 생성해봅시다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FrMt5r%2FbtsDmrSjOLU%2FUXvUS9EKkq6fau5kE19YP0%2Fimg.png)

- 서버 생성 화면

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb30Tjb%2FbtsDkUnzkRL%2FTXEFkfUgTV2LPYHizMThUk%2Fimg.png)

- port와 password는 postgreSQL 을 설치할 때 적었던 것과 동일하게 써야 합니다. port는 건드리지 않았다면 5432 일 것입니다.
- 그리고 그 외 host name이나 username 등은 typeORM 설치 후 설정을 할 때 필요한 것들이니 잊지 않도록 합니다!

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlNZ3t%2FbtsDjYjFuBC%2FhHZMvzZfvDvUFQ6QtxQJ2k%2Fimg.png)

- 서버가 생성되었으면 서버 내에 데이터베이스를 생성합니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FduGOab%2FbtsDjYxdbeb%2F5OTly1Tr9vpVKL9skN52nK%2Fimg.png)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FzMDjs%2FbtsDiQzx8hJ%2FkdAznJ6iu6Far6DTdkmNPK%2Fimg.png)

- 위와 같은 화면이 나온다면 데이터베이스가 정상적으로 생성이 된 것입니다!


# typeORM 세팅

- typeORM은 node.js 진영에서 사용하는 ORM 도구입니다. 자바의 JPA와 같은 역할을 하는 것이죠!
- nest.js에서 정의한 클래스를 관계형 데이터베이스와 연동시켜주어서 개발자가 일일이 테이블을 생성하거나 SQL 쿼리문을 작성하지 않아도 됩니다.

## 필요한 라이브러리 설치

- `npm install pg typeorm @nestjs/typeorm --save`
- postgre(pg), typeorm 모듈과 nest.js에서 typeORM을 연동시키기 위한 모듈(@nestjs/typeorm) 까지 설치해줍니다!


## typeORM 설정

- 설정 정보를 입력하는 방법은 여러가지 방법이 있습니다.

#### app.module.ts에 설정값 바로 넣기

```typescript
// app.module.ts

import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';

@Module({
  imports: [
      TypeOrmModule.forRoot({
        type: 'postgre',
        host: 'localhost',
        port: 5432,
        username: 'admin',
        password: '1234',
        database: 'test',
        entities: [__dirname + '/../**/*.entity{.ts,.js}'],
        synchronize: true,
    }),
  ],
})
export class AppModule {}
```

#### 루트 디렉토리에 json 파일 생성

- 프로젝트의 루트 디렉토리에 json 파일을 생성하면 forRoot() 메소드가 인식을 할 수 있습니다.

```typescript
// ormconfig.json

{
  "type": "postgre",
  "host": "localhost",
  "port": 5432,
  "username": "admin",
  "password": "1234",
  "database": "test",
  "entities": [__dirname + '/../**/*.entity{.ts,.js}'],
  "synchronize": true
}
```

```typescript
// app.module.ts

@Module({
    imports: [TypeOrmModule.forRoot()]
})
```


#### 환경변수(.env) 이용하기

```typescript
// .env

DB_USER=admin
DB_PASSWORD=1234
DB_PORT=5432
DB_HOST=localhost
DB_SCHEMA=test
ENTITY_PATH=__dirname + /../**/*.entity{.ts,.js}
```

```typescript
// ormconfig.js

module.exports = {
  type: 'postgre',
  entities: [process.env.ENTITY_PATH],
  username: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  host: process.env.DB_HOST,
  database: process.env.DB_SCHEMA,
  synchronize: true,
};
```

```typescript
// app.module.ts

@Module({
    imports: [TypeOrmModule.forRoot()]
})
```


#### configService 를 이용하는 방법

`npm i --save @nestjs/config`

- configService를 이용하기 위해서 필요한 패키지를 설치해줍니다.

```typescript
import { Injectable } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';
import { TypeOrmModuleOptions, TypeOrmOptionsFactory } from '@nestjs/typeorm';

@Injectable()
export class MyConfigService implements TypeOrmOptionsFactory {
  constructor(private configService: ConfigService) {}

  createTypeOrmOptions(): TypeOrmModuleOptions {
    return {
      type: 'postgre',
      username: this.configService.get<string>('DB_USER'),
      password: this.configService.get<string>('DB_PASSWORD'),
      port: +this.configService.get<number>('DB_PORT'),
      host: this.configService.get<string>('DB_HOST'),
      database: this.configService.get<string>('DB_SCHEMA'),
      entities: [__dirname + '/../**/*.entity{.ts,.js}'],
    };
  }
}
```

```typescript
// /src/config/database/config.module.ts

import { Module } from '@nestjs/common';
import { MyConfigService } from './config.service';

@Module({
  providers: [MyConfigService],
})
export class MyConfigModule {}
```

```typescript
// app.module.ts

import { Module } from '@nestjs/common';
import { ConfigModule } from '@nestjs/config';
import { TypeOrmModule } from '@nestjs/typeorm';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { MyConfigModule } from './config/database/config.module';
import { MyConfigService } from './config/database/config.service';
import { UserModule } from './user/user.module';

@Module({
  imports: [
    ConfigModule.forRoot({ isGlobal: true }),
    TypeOrmModule.forRootAsync({
      imports: [MyConfigModule],
      useClass: MyConfigService,
      inject: [MyConfigService],
    }),
  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

- 여기서 **ConfigModule.forRoot({ isGlobal: true })** 는 모듈을 전역 모듈로 설정하겠다는 것입니다. 


# Entity 생성

- 이제 데이터베이스의 테이블이 될 엔티티를 생성해보겠습니다.

##### user.entity.ts

```typescript
import { Column, Entity, PrimaryGeneratedColumn } from 'typeorm';

export enum UserRole {
  ADMIN = 'admin',
  MEMBER = 'member',
  GHOST = 'ghost',
}

@Entity()
export class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column('varchar', { length: 15 })
  username: string;

  @Column('int')
  age: number;

  @Column({ type: 'enum', enum: UserRole, default: UserRole.GHOST })
  role: UserRole;
}
```

- 여기서 @PrimaryGeneratedColumn() 은 자동으로 증가하는 primary key를 의미합니다. 스프링부트에서 **@Id, @GeneratedValue(strategy = GenerationType.AUTO)** 어노테이션을 붙여준 것과 같은 데코레이터라고 볼 수 있겠네요!
- 그 외에 테이블 컬럼에는 @Column 데코레이터를 사용해주면 되고 선택적으로 type을 비롯한 옵션들을 넣어줄 수 있습니다.


# Repository 생성

- typeORM 0.3.0 버전부터는 기존에 사용되던 @EntityRepository 데코레이터가 deprecated 되었다고 합니다.
- EntityRepository를 대체할 수 있는 여러 방법들이 있었는데 그 중 custom decorator를 만들어서 사용하는 방법과 datasource의 createEntityManager() 메소드를 이용하는 방법을 정리해보았습니다.

## custom decorator

##### custom repository decorator 생성

```typescript
// db/typeorm-ex.decorator.ts
import { SetMetadata } from "@nestjs/common";

export const TYPEORM_EX_CUSTOM_REPOSITORY = "TYPEORM_EX_CUSTOM_REPOSITORY";

export function CustomRepository(entity: Function): ClassDecorator {
  return SetMetadata(TYPEORM_EX_CUSTOM_REPOSITORY, entity);
}
```

##### 모듈 생성

```typescript
// db/typeorm-ex.module
import { DynamicModule, Provider } from "@nestjs/common";
import { getDataSourceToken } from "@nestjs/typeorm";
import { DataSource } from "typeorm";
import { TYPEORM_EX_CUSTOM_REPOSITORY } from "./typeorm-ex.decorator";

export class TypeOrmExModule {
  public static forCustomRepository<T extends new (...args: any[]) => any>(repositories: T[]): DynamicModule {
    const providers: Provider[] = [];

    for (const repository of repositories) {
      const entity = Reflect.getMetadata(TYPEORM_EX_CUSTOM_REPOSITORY, repository);

      if (!entity) {
        continue;
      }

      providers.push({
        inject: [getDataSourceToken()],
        provide: repository,
        useFactory: (dataSource: DataSource): typeof repository => {
          const baseRepository = dataSource.getRepository<any>(entity);
          return new repository(baseRepository.target, baseRepository.manager, baseRepository.queryRunner);
        },
      });
    }

    return {
      exports: providers,
      module: TypeOrmExModule,
      providers,
    };
  }
}
```

- @CustomRepository 데코레이터가 적용될 repository를 받아줄 모듈입니다.
- `Reflect.getMetadata()` 메서드로 메타데이터 키값인 `TYPEORM_EX_CUSTOM_REPOSITORY`에 해당되는 엔티티를 가져오고,
- 메타데이터 키값에 해당하는 엔티티가 존재하는 경우 Factory를 이용하여 provider를 동적으로 생성하여 providers에 추가하는 역할을 합니다.

##### 모듈 적용

```typescript
@CustomRepository(User)
export class UserRepository extends Repository<User> {
  ...
}
```

##### 모듈 설정

```typescript
@Module({
  imports: [
TypeOrmExModule.forCustomRepository([UserRepository]),
  ],
  ...
})
```


## createEntityManager()

```typescript
// user.repository.ts

import { Injectable } from '@nestjs/common';
import { DataSource, Repository } from 'typeorm';
import { User } from './user.entity';

@Injectable()
export class UserRepository extends Repository<User> {
  constructor(datasource: DataSource) {
    super(User, datasource.createEntityManager());
  }
}
```

- 첫 번째 인자로 사용될 entity를 입력하고 두 번째 인자로 createEntityManager() 를 호출합니다.

```typescript
// user.module.ts

import { Module } from '@nestjs/common';
import { UserController } from './user.controller';
import { UserService } from './user.service';
import { TypeOrmModule } from '@nestjs/typeorm';
import { User } from './user.entity';
import { UserRepository } from './user.repository';

@Module({
  imports: [TypeOrmModule.forFeature([User])],
  controllers: [UserController],
  providers: [UserService, UserRepository],
})
export class UserModule {}
```

- 그리고 모듈에서 providers 안에 repository를 추가해줍니다!

# 데이터베이스 연동 확인

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FTNrjH%2FbtsDlzwEHUN%2FEunkJOdwnDF8ybky15rcVK%2Fimg.png)

- npm start 를 통해 서버를 켜면 typeORM이 자동으로 CREATE 쿼리를 날려서 테이블을 생성하는 것을 볼 수 있습니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fp6r3L%2FbtsDjXZnsDQ%2F2bOCt3aFYn2ghSWvtBRRHk%2Fimg.png)

- pgAdmin 에서도 테이블과 컬럼들이 우리가 만든 Entity에 맞게 생성된 것을 확인할 수 있습니다!