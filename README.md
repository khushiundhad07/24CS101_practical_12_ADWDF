# Practical 12 – CI/CD Pipeline with GitHub Actions

**Student:** Khushi Undhad (24CS101)
**Subject:** ITUE301 – Advanced Web Development Frameworks

## Aim
To automate testing of the Task Manager application using a GitHub Actions CI pipeline.

## Pipeline
The workflow is in `.github/workflows/ci.yml`. It runs on every push and pull request to `main`.

```
push to main
   ↓
Job 1: Lint            → install → ESLint
   ↓ (only if lint passes)
Job 2: Build and Test  → start MongoDB → install → check DB connection → run tests
   ↓
✔ pass  or  ✖ fail
```

## Tests
Tests are in `tests/api.test.js`. They use the Node.js test runner and Supertest.

- Health check returns 200
- Register a user
- Login returns a JWT
- Wrong password is rejected
- `GET /api/tasks` without a token returns 401
- Create a task
- Create a task without a title returns 400
- Get the user's tasks

## Run locally
```bash
npm install
npm run lint
npm test        # needs MongoDB running locally
npm start
```

Create a `.env` file:
```
MONGO_URI=mongodb://127.0.0.1:27017/taskmanager
JWT_SECRET=your_secret
PORT=5000
```
