# 🚀 NesXT DST Template

> Modern full-stack monorepo template powered by **NestJS**, **Next.js**, **Prisma**, and **Turborepo**.

## 📋 Overview

This project is a professional full-stack monorepo template built to quickly bootstrap modern applications with a scalable, maintainable architecture.

Current tech stack:

- 🐣 Backend: NestJS + Prisma (PostgreSQL)
- ⚡ Frontend: Next.js (React)
- 📦 Monorepo: Turborepo + pnpm workspaces
- 🔧 Tooling: TypeScript, ESLint, Prettier

Planned / upcoming:

- 🎨 UI/UX: Tailwind CSS + shadcn/ui
- 📱 Mobile: React Native / Expo support
- 🖥️ Desktop: Electron / Tauri support

---

## 📂 Project architecture

```
.
├── apps/
│   ├── api/              # 🐣 Backend NestJS
│   ├── web/              # 🌐 Main Next.js app
│   └── docs/             # 📖 Documentation Next.js
│
├── packages/
│   ├── database/         # 🗄️ Shared Prisma client (@repo/db)
│   ├── ui/               # 🎨 Shared React components
│   ├── eslint-config/    # ⚙️ ESLint configuration
│   └── typescript-config/# ⚙️ TypeScript configuration
│
├── package.json          # Workspace root config
├── turbo.json            # Turborepo config
└── pnpm-workspace.yaml   # pnpm workspaces config
```

### Applications

| App             | Description                     | Default port |
| --------------- | ------------------------------- | ------------ |
| **`apps/api`**  | NestJS REST API with Prisma ORM | `:8000`      |
| **`apps/web`**  | Main Next.js application        | `:3000`      |
| **`apps/docs`** | Next.js documentation site      | `:3001`      |

### Shared packages

| Package                       | Description                                  |
| ----------------------------- | -------------------------------------------- |
| **`@repo/db`**                | Central Prisma client, schema and migrations |
| **`@repo/ui`**                | Reusable React components                    |
| **`@repo/eslint-config`**     | Shared ESLint configuration                  |
| **`@repo/typescript-config`** | Shared TypeScript configuration              |

---

## 🚀 Quick Start

### Requirements

- Node.js >= 18
- pnpm >= 9.0.0
- PostgreSQL (local or remote)

### Installation

1. Clone the repository:

```bash
git clone <repo-url>
cd nesxt-dst-template
```

2. Install dependencies:

```bash
pnpm install
```

3. Configure the database:

Create a `.env` file at the root or in `packages/database/`:

```bash
DATABASE_URL="postgresql://user:password@localhost:5432/mydb?schema=public"
```

4. Generate the Prisma client:

```bash
pnpm --filter @repo/db db:generate
pnpm --filter @repo/db build
```

5. Run the project in development:

```bash
# Start all services
pnpm dev

# Or start individually
pnpm --filter apps/api dev
pnpm --filter apps/web dev
```

---

## 📚 Module documentation

Each app and package has its own README:

- API Backend: `apps/api/README.md`
- Database: `packages/database/README.md`
- Frontend Web: `apps/web/README.md`

---

## 🛠️ Main scripts

### Development

```bash
pnpm dev              # Start all services in dev mode
pnpm build            # Build all apps and packages
pnpm lint             # Run ESLint
pnpm format           # Format code with Prettier
pnpm check-types      # Check TypeScript types
```

### Database (Prisma)

```bash
pnpm --filter @repo/db db:generate    # Generate Prisma client
pnpm --filter @repo/db db:migrate     # Create/apply migrations
pnpm --filter @repo/db build          # Build the package
```

### Workspace filtering

```bash
pnpm --filter apps/api <command>      # Run a command in the API
pnpm --filter apps/web <command>      # Run a command in Web
pnpm --filter @repo/db <command>      # Run a command in Database
```

---

## 🧰 Detailed tech stack

### Backend (API)

- Framework: NestJS 10+
- ORM: Prisma 6+ with Accelerate
- Database: PostgreSQL
- Testing: Jest + Supertest

### Frontend (Web)

- Framework: Next.js 15+ (App Router)
- Styling: CSS Modules (migrating to Tailwind + shadcn planned)
- State: React hooks (Zustand/Redux planned)

### DevOps & Tooling

- Monorepo: Turborepo (distributed cache, optimized pipelines)
- Package manager: pnpm (workspaces)
- Type safety: TypeScript strict mode
- Code quality: ESLint + Prettier
- Git hooks: Husky + lint-staged (to configure)

---

## 🗺️ Roadmap

### Phase 1: Foundations (✅ Done)

- [x] Turborepo + pnpm workspaces setup
- [x] NestJS backend + Prisma
- [x] Next.js frontend
- [x] Shared database package

### Phase 2: UI/UX (✅ Done)

- [x] Tailwind CSS V4 integration
- [x] shadcn/ui integration

### Phase 3: Multi-platform (🔜 Upcoming)

- [ ] Mobile app (React Native / Expo)
- [ ] Desktop app (Electron / Tauri)
- [ ] Cross-platform code sharing

### Phase 4: Production (Goal)

- [ ] Docker + Docker Compose

---

## 🤝 Contributing

Contributions are welcome. Please:

1. Fork the repo
2. Create a branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'feat: add amazing feature'`)
4. Push the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📝 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## 📞 Support

For questions or issues:

- Open an issue on GitHub
- Check module documentation
- Review app/package READMEs in `apps/` and `packages/`

---

## Author

- [Perret William](https://william-perret.fr)

