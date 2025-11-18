# 🗄️ Database Package — `@repo/db`

> Centralized Prisma package for the NesXT DST Template monorepo.

## 📋 Overview

This package is the single source of truth for PostgreSQL access via Prisma ORM. It exports a configured Prisma client ready to be used by all apps in the monorepo.

Why a dedicated package?

- ✅ Share a single Prisma instance across services
- ✅ Centralize schema and migration management
- ✅ Avoid duplicated configuration
- ✅ Simplify deployment (single Query Engine binary build)

## 🎯 Goals

- Centralize the Prisma client so apps (e.g., `apps/api`) share the same configuration and Query Engine binary.
- Make generate and bundle steps explicit to avoid "Query Engine not found" errors in production.

## 📂 Package structure

```
packages/database/
├─ prisma/
│  └─ schema.prisma              # 📝 Prisma schema (models, relations)
├─ generated/prisma/             # 🤖 Auto-generated client
│  ├─ client.ts
│  ├─ browser.ts
│  ├─ internal/
│  └─ libquery_engine-*.so.node  # ⚙️ Native Query Engine binary
├─ src/
│  └─ client.ts                  # 🔧 Singleton wrapper + Accelerate
├─ index.ts                      # 📦 Package entrypoint
├─ package.json                  # 📋 Package config (@repo/db)
├─ tsconfig.json
└─ .env                          # 🔐 Environment variables (local)
```

## 🛠️ Available scripts

| Command            | Description                                       |
| ------------------ | ------------------------------------------------- |
| `pnpm db:generate` | Generates the Prisma client (`prisma generate`)   |
| `pnpm db:migrate`  | Creates and applies migrations in dev             |
| `pnpm build`       | Build TypeScript + copy Query Engine into `dist/` |
| `pnpm dev`         | Watch mode: generate + build automatically        |

## ⚙️ Configuration

### Required environment variables

Create a `.env` file in `packages/database/` or at the repo root:

```bash
DATABASE_URL="postgresql://user:password@localhost:5432/mydb?schema=public"
```

> ⚠️ Important: `DATABASE_URL` is required at runtime for Prisma to connect.

## 🚀 Usage

### Development

Step 1: Install workspace dependencies

```bash
pnpm install
```

Step 2: Generate the Prisma client

```bash
pnpm --filter @repo/db db:generate
```

Step 3: Run watch mode (optional)

```bash
pnpm --filter @repo/db dev
```

Verify the Query Engine binary exists:

```bash
ls packages/database/generated/prisma/libquery_engine-*.so.node
```

### Build for CI / Production

Step 1: Generate the client and build the package

```bash
pnpm --filter @repo/db db:generate
pnpm --filter @repo/db build
```

Step 2: Verify the Query Engine is in `dist/`

```bash
ls packages/database/dist/generated/prisma/libquery_engine-*.so.node
```

> 💡 Note: If only a `.tmp` file is present, re-run `db:generate` until the final binary appears.

---

## 📦 Integration in apps

### Import the Prisma client

In any service in the monorepo:

```ts
import { prisma } from "@repo/db";

// Example: fetch all users
const users = await prisma.user.findMany();
```

### Full example (NestJS)

```ts
import { Injectable } from "@nestjs/common";
import { prisma } from "@repo/db";

@Injectable()
export class UserService {
  async findAll() {
    return prisma.user.findMany();
  }

  async findOne(id: string) {
    return prisma.user.findUnique({ where: { id } });
  }
}
```

## 🔍 Troubleshooting

### ❌ Error: "Prisma Client could not locate the Query Engine"

Cause: The native binary `libquery_engine-*.so.node` is missing or in the wrong place.

Solutions:

1. Check for the binary:
   ```bash
   ls packages/database/generated/prisma/libquery_engine-*.so.node
   ```
2. Regenerate the client:
   ```bash
   pnpm --filter @repo/db db:generate
   ```
3. Build the package:
   ```bash
   pnpm --filter @repo/db build
   ```
4. Ensure `dist/generated/prisma` contains the binary after build.

### ❌ Error: "Environment variable not found: DATABASE_URL"

Cause: `DATABASE_URL` is not defined.

Solutions:

1. Create a `.env` with `DATABASE_URL`
2. Or export the variable before starting the app:
   ```bash
   export DATABASE_URL="postgresql://user:pass@localhost:5432/db?schema=public"
   ```

---

## ✅ Best practices

Schema change workflow

1. Edit `prisma/schema.prisma`
2. Create a migration:
   ```bash
   pnpm --filter @repo/db db:migrate
   ```
3. Generate the client:
   ```bash
   pnpm --filter @repo/db db:generate
   ```
4. Build the package:
   ```bash
   pnpm --filter @repo/db build
   ```

Rules

- ✅ Always import `prisma` from `@repo/db`
- ❌ Never instantiate a new `PrismaClient` in apps
- ✅ Use the shared singleton to avoid multiple connections
- ✅ Run `db:generate` after each schema change

---

## 📚 Quick reference

Commands from the repo root

```bash
# Install
pnpm install

# Generate Prisma client
pnpm --filter @repo/db db:generate

# Build the package
pnpm --filter @repo/db build

# Create a migration
pnpm --filter @repo/db db:migrate

# Run dev (watch)
pnpm --filter @repo/db dev

# Run the API with DATABASE_URL
DATABASE_URL="postgresql://user:pass@localhost:5432/db" pnpm --filter apps/api dev
```

### Useful links

- 📖 https://www.prisma.io/docs
- 🚀 https://www.prisma.io/docs/accelerate
- 📦 ../../README.md
- 🐣 ../../apps/api/README.md

---

Package maintained by: [Perret William](https://william-perret.fr)  
Last updated: November 2025
