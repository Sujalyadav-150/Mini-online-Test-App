# QuizMaster – Mini Online Test App (MERN)

A full-stack online test application with student authentication, a 10-question test, a 10-minute timer, server-side scoring, results review, attempt history, and a basic admin question-creation API.

## Stack
- React + Vite
- Node.js + Express
- MongoDB + Mongoose
- JWT + bcryptjs

## Requirements
Node.js 18+, npm, and a MongoDB database (local MongoDB or MongoDB Atlas).

## 1. Configure the backend

```bash
cd backend
cp .env.example .env
npm install
```

Set `MONGO_URI`, `JWT_SECRET`, and (optionally) `CLIENT_URL` in `backend/.env`.

Seed 10 sample questions:

```bash
npm run seed
```

Start the API:

```bash
npm run dev
```

The API runs at `http://localhost:5000`.

## 2. Configure the frontend

Open a second terminal:

```bash
cd frontend
cp .env.example .env
npm install
npm run dev
```

Open the Vite URL shown in the terminal (usually `http://localhost:5173`).

## Admin bonus
To create an admin, register normally and then set that user's `role` to `admin` in MongoDB (`users` collection). Admin users will see the Admin page and can add questions.

## API overview
- `POST /api/auth/register` – create account
- `POST /api/auth/login` – login
- `GET /api/questions` – get 10 questions (correct answers are not exposed)
- `POST /api/results/submit` – submit answers; score is calculated on server
- `GET /api/results/history` – signed-in user's attempts
- `GET /api/results/:id` – view an attempt owned by the signed-in user
- `POST /api/questions` – admin-only question creation

## Deployment notes
- Deploy `backend` as a Node web service (Render, Railway, etc.) and configure `MONGO_URI`, `JWT_SECRET`, and `CLIENT_URL`.
- Deploy `frontend` as a static site (Vercel or Netlify). Set `VITE_API_URL` to the deployed backend URL plus `/api`, for example `https://your-api.onrender.com/api`.
- Add your frontend domain to the backend `CLIENT_URL` setting.
- The frontend and backend are separate deployables; configure environment variables on each platform.

## Deploying both apps to Vercel
Create two Vercel projects from this repository:

1. For the API, set the project's Root Directory to `backend`. Vercel detects the Express app. Add `MONGO_URI` (a reachable MongoDB Atlas URI, not a localhost URI), `JWT_SECRET`, and `CLIENT_URL` (the frontend's deployed origin).
2. For the frontend, set the Root Directory to `frontend`. Add `VITE_API_URL` with the API deployment URL followed by `/api`, such as `https://your-api.vercel.app/api`. The `vercel.json` rewrite supports client-side routes such as `/login` and `/history`.

Deploy the frontend once to obtain its URL, then set that origin as the backend's `CLIENT_URL`. After deploying the backend, set `VITE_API_URL` to its URL and redeploy the frontend.

## Important implementation notes
- Passwords are hashed with bcrypt.
- API routes are protected with JWT.
- The correct answer is never included in the question-list response.
- The backend calculates the score from stored question answers; do not trust a score sent by the browser.
- The backend enforces the 10-minute attempt duration using a server-side `startedAt` timestamp.
- This is an assignment starter. Before production use, add rate limiting, stronger validation, HTTPS-only deployment, refresh-token/session strategy, and automated tests.
