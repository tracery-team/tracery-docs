# Application Setup Documentation

This document provides a step-by-step guide to setting up the application using NestJS. The setup involves configuring the main application module and the main entry file.

## Prerequisites

Ensure you have the following installed:

- Node.js
- npm or yarn
- PostgreSQL

## Environment Variables

Create a `.env` file in the root directory of your project and add the following environment variables:

```plaintext
DATABASE_HOST=your_database_host
DATABASE_PORT=your_database_port
DATABASE_USER=your_database_user
DATABASE_PASSWORD=your_database_password
DATABASE_NAME=your_database_name
PORT=your_application_port
```

## App Module Configuration

The `AppModule` is the root module of the application. It imports and configures other modules and services.

### app.module.ts

```typescript
import { Module } from "@nestjs/common";
import { AuthModule } from "./modules/auth/auth.module";
import { TypeOrmModule } from "@nestjs/typeorm";
import { UserEntity } from "./data/user.entity";
import { ProfileModule } from "./modules/profile/profile.module";
import { EventModule } from "./modules/event/event.module";
import { UserModule } from "./modules/user/user.module";
import { EventEntity } from "./data/event.entity";

@Module({
  imports: [
    TypeOrmModule.forRoot({
      type: "postgres",
      host: process.env.DATABASE_HOST,
      port: Number(process.env.DATABASE_PORT),
      username: process.env.DATABASE_USER,
      password: process.env.DATABASE_PASSWORD,
      database: process.env.DATABASE_NAME,
      entities: [UserEntity, EventEntity],
      synchronize: true, // TODO: migrations
    }),
    AuthModule,
    EventModule,
    UserModule,
    ProfileModule,
  ],
})
export class AppModule {}
```

### Explanation

- **TypeOrmModule.forRoot**: Configures the TypeORM module to connect to a PostgreSQL database using environment variables.
- **Entities**: Specifies the entities used in the application (`UserEntity` and `EventEntity`).
- **Modules**: Imports other feature modules (`AuthModule`, `EventModule`, `UserModule`, `ProfileModule`).

## Main Entry File

The `main.ts` file is the entry point of the application. It bootstraps the NestJS application and applies global configurations.

### main.ts

```typescript
import "dotenv/config";
import { NestFactory } from "@nestjs/core";
import { AppModule } from "./app.module";
import { ValidationPipe } from "@nestjs/common";

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(
    new ValidationPipe({
      whitelist: true,
      transform: true,
    })
  );
  await app.listen(process.env.PORT ?? 3000);
}
bootstrap();
```

### Explanation

- **dotenv/config**: Loads environment variables from the `.env` file.
- **NestFactory.create**: Creates a new NestJS application instance using the `AppModule`.
- **ValidationPipe**: Applies global validation pipes to automatically validate incoming requests.
- **app.listen**: Starts the application on the specified port (default is 3000).

## Running the Application

To run the application, use the following command:

```bash
npm run start
```

or

```bash
yarn start
```

Ensure your PostgreSQL database is running and the environment variables are correctly set up.
