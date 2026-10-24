# Practical 12 – CI Pipeline with GitHub Actions

![CI Pipeline](https://github.com/khushiundhad07/24CS101_practical_12_ADWDF/actions/workflows/ci.yml/badge.svg)

**Student:** Khushi Undhad (24CS101) · **Subject:** ITUE301 Advanced Web Development Frameworks

Task Manager API (Express + MongoDB + JWT) with a Continuous Integration pipeline.
Every push and pull request to `main` is automatically linted and tested.

## Pipeline

```
push / PR to main
      │
      ▼
┌─────────────┐  needs   ┌──────────────────────────────┐
│  lint job   │ ───────► │  build-and-test job          │
│  npm ci     │          │  MongoDB service (mongo:7)   │
│  eslint .   │          │  npm ci → npm test (Jest)    │
└─────────────┘          └──────────────────────────────┘
```

- **lint** – runs ESLint on the backend. If it fails, tests do not run.
- **build-and-test** – starts a MongoDB service container, installs dependencies and runs Jest + Supertest.

## Tests (`tests/api.test.js`)

| Endpoint | Check |
|---|---|
| `GET /api/health` | 200, status ok |
| `POST /api/auth/register` | 201, user created |
| `POST /api/auth/login` | 200 with JWT; 401 on wrong password |
| `GET /api/tasks` | 401 without token; 200 with the user's tasks |
| `POST /api/tasks` | 201 with title; 400 without title |

## Project structure changes

- `app.js` – builds and **exports** the Express app (no `listen`), so tests can import it.
- `server.js` – connects to MongoDB and starts the server.
- `eslint.config.js` – ESLint flat config.
- `.github/workflows/ci.yml` – the pipeline.

## Run locally

```bash
npm install
npm run lint
# needs a local MongoDB
MONGO_URI=mongodb://127.0.0.1:27017/taskmanager_test JWT_SECRET=test npm test
npm start          # uses .env (MONGO_URI, JWT_SECRET, PORT)
```
