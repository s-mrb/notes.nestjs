
# Index
- [Index](#index)
- [Intro](#intro)
- [Installating Nest, Creating and Running Project](#installating-nest-creating-and-running-project)
- [Components of  a Nest Application](#components-of--a-nest-application)
- [Cont](#cont)

# Intro

NodeJS makes no assumption and includes almost nothing by default, for it's purposely meant  to be very bare bones. NodeJS by design have a minimalistic setup and developers are incharge of setting up everything they want to use in for their application.
This applies to eveything, how you handle:
- routing
- API calls
- set up web sockets
- code organization
- name conventions

This flexibility can be bit of a double edged sword, creating potential problems as applications or teams grow very large.

> NestJS solves all of these potential problems by providng a abstraction or overall framework around NodeJS. Letting you focus on the application and not on the tiny details NestJS provides out of the box application architecture.

NestJS provides you architecture/modules that is/are:
- platform agnostic
- HTTP framwork agnostic
  - express
  - fastify
  - etc

> With the help of dependency injection we can swap out the underlying mechanism effortlessly.

# Installating Nest, Creating and Running Project


```
<!-- install -->
npm i -g @nestjs/cli

<!-- new nest project -->
nest new

<!-- run project -->
nest start

<!-- Served on port 3000 -->
```

# Components of  a Nest Application

![1](static/images/1.png)

- Core of nest app lives in `src` directory
- Entire application starts with the `main.ts`

**main.ts**
```ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();

```

- Inside `main.ts` you can see that entire application is created via `NestFactory.create` taking in an `AppModule`
- `AppModule` is root module that contains everything that our app needs to run

**app.module.ts**

```ts
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

- Decorators are simply functions that apply logic, they can be applied to methods, functions, classes and even parameters
- Decorator of `AppModule` encapsulates everything that is important in that modules context
  - **controllers**
  - **providers**


- Controllers are where specific **requests** of your application are handles, in below controller **Get** request is handled

**app.controller.ts**
```ts
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

> You can see above that `AppController` utilizes `AppService` **Provider** to separate out business logic from the controller itself, **Provider**s like this work for **dependency injection**.

- `AppController` gets initialized with servie (***this is a class***) named `appService`  initialized with `AppService`
- `AppService` class contains method `getHello`

**app.service.ts**

```ts
import { Injectable } from '@nestjs/common';

@Injectable()
export class AppService {
  getHello(): string {
    return 'Hello World!';
  }
}

```


> You can see **service** is made **injectable**, to allow **dependency injection** and to **separate business logic from the controller** itself.



# Cont

- add dotenv
- add port on console when we run project