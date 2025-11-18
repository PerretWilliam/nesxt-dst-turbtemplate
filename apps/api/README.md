# 🐣 API Backend — NestJS

> REST API backend service built with **NestJS** and **Prisma**.

## 📋 Overview

The API backend of the NesXT DST Template project exposes REST endpoints for frontend apps (web, mobile, desktop). It uses the centralized Prisma client provided by `@repo/db`.

Tech stack:

- 🐣 **Framework**: NestJS 10+
- 🗄️ **ORM**: Prisma via `@repo/db`
- 🔐 **Auth**: To be implemented (JWT, Passport)
- ✅ **Testing**: Jest + Supertest
- 📝 **Validation**: class-validator + class-transformer

## 🎯 Purpose of this document

- ✅ Explain how to start the service in development
- ✅ Describe required environment variables
- ✅ Document integration with `@repo/db`
- ✅ Provide code examples

## 📂 Project structure

```
apps/api/
├─ src/
│  ├─ main.ts                    # 🚀 Bootstrap NestJS
│  ├─ app.module.ts              # 📦 Root module
│  ├─ app.controller.ts          # 🛣️ Example endpoints
│  ├─ app.service.ts             # 💼 Business logic
│  └─ prisma/
│     ├─ prisma.module.ts        # 📦 Prisma module
│     └─ prisma.service.ts       # 🔧 PrismaService (wrapper around @repo/db)
├─ test/
│  ├─ app.e2e-spec.ts           # 🧪 E2E tests
│  └─ jest-e2e.json
├─ .env.example                  # 📝 Environment variables template
├─ package.json
└─ tsconfig.json
```

## ⚙️ Configuration

### Environment variables

Create a `.env` file in `apps/api/` (or at the repo root):

```bash
# Database
DATABASE_URL="postgresql://user:password@localhost:5432/mydb?schema=public"

# Server
PORT=8000
NODE_ENV=development
```

## 🚀 Quick start

### Prerequisites

- Node.js >= 18
- pnpm >= 9.0.0
- PostgreSQL running
- `@repo/db` package generated and built

### Install and run

Step 1: Install dependencies (from the monorepo root)

```bash
pnpm install
```

Step 2: Prepare the database package

```bash
pnpm --filter @repo/db db:generate
pnpm --filter @repo/db build
```

Step 3: Configure environment variables

```bash
# Copy the template
cp apps/api/.env.example apps/api/.env

# Edit with your values
nano apps/api/.env
```

Step 4: Start the API in dev mode

```bash
pnpm --filter apps/api dev
```

Step 5: Verify the API responds

```bash
curl http://localhost:8000
# or open in your browser
```

### Available commands

| Command         | Description                                 |
| --------------- | ------------------------------------------- |
| `pnpm dev`      | Start the server in watch mode (hot-reload) |
| `pnpm build`    | Build the application for production        |
| `pnpm start`    | Start the server in production mode         |
| `pnpm test`     | Run unit tests                              |
| `pnpm test:e2e` | Run E2E tests                               |
| `pnpm lint`     | Lint the code with ESLint                   |

## 🔌 Integration with `@repo/db`

### Architecture

The API uses the shared Prisma client provided by the `@repo/db` package. This ensures:

- ✅ A single Prisma instance (singleton pattern)
- ✅ No multiple DB connections
- ✅ Centralized configuration

### Usage in services

Option 1: Direct import (recommended for simple cases)

```ts
import { Injectable } from '@nestjs/common';
import { prisma } from '@repo/db';

@Injectable()
export class UserService {
  async findAll() {
    return prisma.user.findMany();
  }

  async findOne(id: string) {
    return prisma.user.findUnique({ where: { id } });
  }

  async create(data: CreateUserDto) {
    return prisma.user.create({ data });
  }
}
```

Option 2: Via PrismaService (for dependency injection)

```ts
import { Injectable } from '@nestjs/common';
import { PrismaService } from './prisma/prisma.service';

@Injectable()
export class UserService {
  constructor(private prisma: PrismaService) {}

  async findAll() {
    return this.prisma.user.findMany();
  }
}
```

### Full example: User CRUD

```ts
// user.controller.ts
import { Controller, Get, Post, Body, Param } from '@nestjs/common';
import { UserService } from './user.service';

@Controller('users')
export class UserController {
  constructor(private readonly userService: UserService) {}

  @Get()
  findAll() {
    return this.userService.findAll();
  }

  @Get(':id')
  findOne(@Param('id') id: string) {
    return this.userService.findOne(id);
  }

  @Post()
  create(@Body() createUserDto: CreateUserDto) {
    return this.userService.create(createUserDto);
  }
}
```

---

## 📝 Operational notes

### Development

- Hot-reload: The server restarts automatically on file changes
- Prisma sync: Keep `@repo/db` in dev mode to regenerate the client automatically:
  ```bash
  pnpm --filter @repo/db dev
  ```

### Production / CI

- Always build `@repo/db` before building the API:
  ```bash
  pnpm --filter @repo/db build
  pnpm --filter apps/api build
  ```

## 🔍 Troubleshooting

### ❌ Error: "Environment variable not found: DATABASE_URL"

Solution: Set `DATABASE_URL` in `.env` or export it:

```bash
export DATABASE_URL="postgresql://user:pass@localhost:5432/db"
```

### ❌ Error: "Prisma Client could not locate the Query Engine"

Solutions:

1. Ensure `@repo/db` has been built:
   ```bash
   pnpm --filter @repo/db build
   ```
2. Check for the binary:
   ```bash
   ls packages/database/dist/generated/prisma/libquery_engine-*.so.node
   ```

### ❌ Error: "Cannot find module '@repo/db'"

Solution: Reinstall dependencies:

```bash
pnpm install
```

### 🐛 Port already in use

Solution: Change the port in `.env` or kill the process:

```bash
lsof -ti:8000 | xargs kill -9
```

---

## 📚 Resources

### Documentation

- 🐣 NestJS Documentation: https://docs.nestjs.com/
- 🗄️ Prisma Documentation: https://www.prisma.io/docs
- 📦 Package Database (@repo/db): ../../packages/database/README.md
- 🏠 Main README: ../../README.md

### Project locations

- Prisma schema: `packages/database/prisma/schema.prisma`
- NestJS configuration: `nest-cli.json`
- E2E tests: `test/app.e2e-spec.ts`

---

## 🚧 Roadmap

- [ ] Health checks
- [ ] Docker support

---

Service maintained by: [Perret William](https://william-perret.fr)  
Last updated: November 2025
