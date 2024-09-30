<h1 style="font-size: 48px;">TypeORM</h1>


<h2> 1. `Entity`: 엔티티는 데이터베이스 테이블과 매핑되는 클래스. 데코레이터를 사용해서 엔티티 정의.</h2>

```typescript
// src/users/user.entity.ts

import { Entity, Column, PrimaryGeneratedColumn, Unique, OneToMany } from 'typeorm';
import { Post } from '../posts/post.entity'; // 예시 관계

@Entity()
@Unique(['email']) // 이메일은 유일해야 함
export class User {
  @PrimaryGeneratedColumn('uuid')
  id: string;

  @Column({ nullable: true })
  googleId: string;

  @Column({ nullable: true })
  facebookId: string;

  @Column()
  email: string;

  @Column()
  name: string;

  @Column({ nullable: true })
  avatar: string;

  @OneToMany(() => Post, post => post.user)
  posts: Post[];
}
```
@Entity(): 이 클래스가 데이터베이스 테이블과 매핑됨을 나타냄.  
@Unique(['email']): email 필드가 유일해야 함을 지정.   
@PrimaryGeneratedColumn('uuid'): 기본 키로 UUID를 사용.  
@Column(): 데이터베이스 컬럼을 정의. nullable: true는 해당 컬럼이 NULL을 허용함을 의미.  
@OneToMany(() => Post, post => post.user): 일대다 관계를 설정. 한 사용자가 여러 개의 포스트를 가질 수 있음.  

<h2>2. 리포지토리(Repository)와 매니저(EntityManager)</h2>

TypeORM에서는 **리포지토리(Repository)**와 **매니저(EntityManager)**를 통해 데이터베이스와 상호작용합니다.  
리포지토리는 특정 엔티티에 대한 데이터 접근을 담당합니다. NestJS에서는 @InjectRepository 데코레이터를 사용하여 리포지토리를 주입받을 수 있습니다.

```typescript
// src/users/users.service.ts

import { Injectable } from '@nestjs/common';
import { Repository } from 'typeorm';
import { InjectRepository } from '@nestjs/typeorm';
import { User } from './user.entity';

@Injectable()
export class UsersService {
  constructor(
    @InjectRepository(User)
    private usersRepository: Repository<User>,
  ) {}

  // 사용자 찾기
  async findOneByEmail(email: string): Promise<User | undefined> {
    return this.usersRepository.findOne({ where: { email } });
  }

  async findOneById(id: string): Promise<User | undefined> {
    return this.usersRepository.findOne({ where: { id } });
  }

  // 새로운 사용자 생성
  async create(userData: Partial<User>): Promise<User> {
    const user = this.usersRepository.create(userData);
    return this.usersRepository.save(user);
  }

  // 사용자 업데이트, 삭제 등 추가 메서드...
}

```

<h3>매니저(EntityManager)</h3>

EntityManager는 전체 데이터베이스에 대한 관리 기능을 제공하며, 특정 엔티티에 종속되지 않습니다. 여러 엔티티에 걸친 작업을 수행할 때 유용합니다.

```typescript
// src/some.service.ts

import { Injectable } from '@nestjs/common';
import { EntityManager } from 'typeorm';
import { InjectEntityManager } from '@nestjs/typeorm';

@Injectable()
export class SomeService {
  constructor(
    @InjectEntityManager()
    private entityManager: EntityManager,
  ) {}

  async someOperation() {
    const user = await this.entityManager.findOne(User, { where: { id: 'some-id' } });
    // 다른 엔티티 작업...
  }
}

```


1. Nest에서 DB연결해주는 것 -> 객체의 데이터 - 연결 - DB의 데이터
2. ORM을 통해 객체간의 관계를 바탕으로 SQL을 자동으로 생성해줌
3. 단점: DB가 복잡해지면 SQL생성 최적화가 약해짐
4. 연관관계에 대해 알아보자
5. Repository
6. Active Record 및 Data Mapper 패턴 지원
7. 쿼리 빌더
8. .env에 환경 변수 등록
```
TYPEORM_CONNECTION=postgres
TYPEORM_HOST=localhost
TYPEORM_PORT=5432
TYPEORM_USERNAME=your_db_username
TYPEORM_PASSWORD=your_db_password
TYPEORM_DATABASE=your_db_name
TYPEORM_SYNCHRONIZE=true
TYPEORM_LOGGING=true
```
9. App Module에 설정 
```typescript
// src/app.module.ts

import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { ConfigModule, ConfigService } from '@nestjs/config';
import { UsersModule } from './users/users.module'; // 예시 모듈
import { AuthModule } from './auth/auth.module'; // 예시 모듈

@Module({
  imports: [
    // 환경 변수 관리
    ConfigModule.forRoot({
      isGlobal: true, // 전역으로 사용 가능하게 설정
    }),
    // TypeORM 설정
    TypeOrmModule.forRootAsync({
      imports: [ConfigModule],
      inject: [ConfigService],
      useFactory: (configService: ConfigService) => ({
        type: configService.get<string>('TYPEORM_CONNECTION') as any,
        host: configService.get<string>('TYPEORM_HOST'),
        port: configService.get<number>('TYPEORM_PORT'),
        username: configService.get<string>('TYPEORM_USERNAME'),
        password: configService.get<string>('TYPEORM_PASSWORD'),
        database: configService.get<string>('TYPEORM_DATABASE'),
        entities: [__dirname + '/**/*.entity{.ts,.js}'],
        synchronize: configService.get<boolean>('TYPEORM_SYNCHRONIZE'),
        logging: configService.get<boolean>('TYPEORM_LOGGING'),
      }),
    }),
    // 다른 모듈들
    UsersModule,
    AuthModule,
  ],
  controllers: [],
  providers: [],
})
export class AppModule {}
```

10. DataSource : `DataSource`는 데이터 베이스 연결 및 설정을 관리하는 역할을 합니다.

11. typeorm model generator:  DB -> orm code 만들어줌