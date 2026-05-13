# MindSync Backend

> Gamified onchain knowledge quiz platform — test your mind, challenge others, dominate tournaments.

MindSync is a Web3 quiz platform where players earn rewards, build reputation, and compete onchain across three game modes. This repository contains the NestJS REST API that powers the platform.

---

## Game Modes

### 🧠 Quiz Master (Solo)
Individual play against a curated question bank. Players earn XP and onchain score attestations based on accuracy and speed. Perfect for daily practice and leaderboard climbing.

### ⚔️ Challenger Mode (1v1)
Two players enter a real-time head-to-head quiz duel. Both answer the same questions simultaneously; the faster and more accurate player wins. Results and wagers are settled onchain.

### 🏆 Tournament Mode (Multi-player)
Bracket or round-robin tournaments with multiple participants. Entry fees, prize pools, and final standings are managed via smart contracts. The last mind standing takes the pot.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Runtime | Node.js 20+ |
| Framework | NestJS 10 |
| Language | TypeScript 5 |
| ORM | TypeORM 0.3 |
| Database | MySQL 8 |
| Validation | class-validator + class-transformer |

---

## Project Structure

```
src/
├── config/
│   └── database.config.ts   # TypeORM MySQL connection factory
├── users/
│   ├── user.entity.ts
│   ├── users.service.ts
│   ├── users.controller.ts
│   └── users.module.ts
├── app.module.ts
└── main.ts
```

New feature modules (quizzes, challenges, tournaments, questions, leaderboard, etc.) follow the same pattern: `entity → service → controller → module`, then imported into `AppModule`.

---

## Getting Started

### Prerequisites

- Node.js 20+
- MySQL 8 running locally or via Docker
- A `.env` file (see below)

### Installation

```bash
cd backend
npm install
```

### Environment

```bash
cp .env.example .env
```

Edit `.env` with your values:

```env
PORT=3000
NODE_ENV=development

DB_HOST=localhost
DB_PORT=3306
DB_USERNAME=root
DB_PASSWORD=password
DB_NAME=mindsync
```

### Running

```bash
# development (watch mode)
npm run start:dev

# production build
npm run build
npm run start:prod
```

The API is available at `http://localhost:3000/api`.

---

## API Overview

All routes are prefixed with `/api`.

| Method | Route | Description |
|---|---|---|
| GET | `/api/users` | List all users |
| GET | `/api/users/:id` | Get a user by ID |
| POST | `/api/users` | Create a user |
| PUT | `/api/users/:id` | Update a user |
| DELETE | `/api/users/:id` | Delete a user |

Quiz, challenge, tournament, leaderboard, and onchain endpoints will be added as their modules are built out.

---

## Database

TypeORM is configured with `synchronize: true` in development — tables are auto-created from entities. **Do not use `synchronize` in production.** Use migrations instead:

```bash
# generate a migration after entity changes
npx typeorm migration:generate src/migrations/MigrationName -d dist/config/database.config.js

# run pending migrations
npx typeorm migration:run -d dist/config/database.config.js
```

---

## Adding a New Module

```bash
# scaffold with NestJS CLI
npx nest g module quizzes
npx nest g service quizzes
npx nest g controller quizzes
```

Then create `quiz.entity.ts`, register it in `QuizzesModule` via `TypeOrmModule.forFeature([Quiz])`, and import `QuizzesModule` in `AppModule`.

---

## Scripts

| Command | Description |
|---|---|
| `npm run start:dev` | Start with hot reload |
| `npm run build` | Compile to `dist/` |
| `npm run start:prod` | Run compiled output |
| `npm run lint` | Run ESLint |
| `npm run test` | Run unit tests |

---

## Onchain Integration

Smart contract interactions (score attestations, wager settlement, tournament prize pools) will be handled via a dedicated `blockchain` module using ethers.js or viem. Contract addresses and RPC URLs will be injected through environment variables.

---

## License

MIT
