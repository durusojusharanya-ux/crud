# Task Management API — Full-Stack Deployment & REST API

[![Node.js](https://img.shields.io/badge/Node.js-18%2B%20%7C%2022%2B-green?logo=node.js)](https://nodejs.org/)
[![Express.js](https://img.shields.io/badge/Express-4.18-lightgrey?logo=express)](https://expressjs.com/)
[![PostgreSQL](https://img.shields.io/badge/Database-PostgreSQL%20%2F%20SQLite-blue?logo=postgresql)](https://www.postgresql.org/)
[![JWT](https://img.shields.io/badge/Auth-JWT-orange?logo=jsonwebtokens)](https://jwt.io/)
[![Deployment](https://img.shields.io/badge/Deployed-Render-brightgreen?logo=render)](https://render.com/)

A production-quality CRUD REST API with JWT User Authentication and persistent hosted database support, designed and prepared for **Internship Task 3 — Full-Stack Deployment**.

---

## 🌟 Features

- 🔐 **JWT User Authentication**: Secure registration, login, and `/api/auth/me` profile lookup.
- 🗄️ **Hosted PostgreSQL & Local SQLite**: Connects to cloud PostgreSQL (`DATABASE_URL`) on Render/Neon/Supabase with SSL, and automatically falls back to SQLite for local offline development.
- 🔄 **Safe Auto-Migration**: Verifies and provisions database schema on startup with foreign key cascades and constraints.
- ✅ **Complete Task CRUD**: Create, Read, Update (full/partial), and Delete tasks with strict user ownership isolation.
- ⏱️ **Activity Audit Timeline**: Tracks task creation, status updates, edits, and deletions via `/api/activities`.
- 🩺 **Health-Check Endpoint**: `GET /api/health` reports uptime, timestamp, and active database engine.
- 🛡️ **Production Security**: Parameterized queries to prevent SQL injection, bcrypt password hashing (12 salt rounds), dynamic CORS whitelist for Vercel and localhost.
- 🚀 **Cloud-Ready**: Dynamic host and port binding (`0.0.0.0:${PORT}`), graceful shutdown on `SIGTERM`, Render Blueprint (`render.yaml`).

---

## 📋 API Endpoints Reference

### Public Endpoints
| Method | Endpoint | Description |
| :--- | :--- | :--- |
| `GET` | `/` | API root metadata and documentation |
| `GET` | `/api/health` | Health check & database connection status |
| `POST` | `/api/auth/register` | Register new user account |
| `POST` | `/api/auth/login` | Authenticate and obtain JWT token |

### Protected Endpoints (Requires `Authorization: Bearer <token>`)
| Method | Endpoint | Description |
| :--- | :--- | :--- |
| `GET` | `/api/auth/me` | Retrieve authenticated user profile |
| `GET` | `/api/tasks` | Get all tasks belonging to user (supports `?status=&priority=`) |
| `POST` | `/api/tasks` | Create a new task |
| `GET` | `/api/tasks/:id` | Get single task details |
| `PUT` | `/api/tasks/:id` | Update task fields (status, priority, dueDate, title, description) |
| `DELETE` | `/api/tasks/:id` | Delete task |
| `GET` | `/api/activities` | Get recent user activity audit trail |

---

## ⚙️ Environment Variables

Configure the following environment variables on your hosting platform (Render):

| Variable | Required in Production | Example / Default | Description |
| :--- | :--- | :--- | :--- |
| `PORT` | Managed by Render | `10000` or `5000` | Port for the HTTP server to listen on |
| `NODE_ENV` | Yes | `production` | Environment mode |
| `JWT_SECRET` | Yes | `a_secure_random_string_min_32_chars` | Secret key for signing JWT tokens |
| `JWT_EXPIRES_IN`| No | `7d` | Token expiry duration |
| `DATABASE_URL` | **Yes (PostgreSQL)** | `postgresql://user:pass@host:5432/dbname?sslmode=require` | Hosted PostgreSQL connection string (Neon / Render Postgres / Supabase) |
| `FRONTEND_URL` | Recommended | `https://your-frontend.vercel.app` | Allowed CORS origin for the Vercel frontend |

> ⚠️ **Security Warning**: Never commit actual database connection strings or JWT secrets to Git. Always configure them in platform environment settings.

---

## 🚀 Deployment Guide: Backend on Render

### Option A: Using Render Blueprint (Recommended — 1-Click Provisioning)
1. Fork or push this repository to your GitHub account: `https://github.com/durusojusharanya-ux/crud`.
2. Log in to [Render](https://dashboard.render.com/).
3. Click **New +** ➔ **Blueprint**.
4. Select your `crud` repository.
5. Render reads `render.yaml` and automatically provisions:
   - A **Free PostgreSQL Database** (`taskflow-postgres`).
   - A **Node.js Web Service** (`taskflow-crud-api`).
   - Automatically injects `DATABASE_URL` and creates a random `JWT_SECRET`!
6. Click **Apply**. Once built, your backend URL will be live at `https://<your-service-name>.onrender.com`.

### Option B: Manual Web Service Setup
1. In the Render Dashboard, click **New +** ➔ **Web Service**.
2. Connect your GitHub repository: `durusojusharanya-ux/crud`.
3. Fill in the following settings:
   - **Name**: `taskflow-crud-api`
   - **Environment**: `Node`
   - **Branch**: `main`
   - **Build Command**: `npm install`
   - **Start Command**: `npm start` (or `node src/server.js`)
   - **Instance Type**: `Free`
4. Under **Environment Variables**, add:
   - `NODE_ENV` = `production`
   - `JWT_SECRET` = `<any secure random 32+ character string>`
   - `DATABASE_URL` = `<Your hosted PostgreSQL URL from Neon or Render>`
   - `FRONTEND_URL` = `<Your Vercel frontend domain>`
5. Click **Create Web Service**.

---

## 🗄️ Hosted PostgreSQL Setup (Free Tier)

### Free Option 1: Neon (neon.tech)
1. Go to [neon.tech](https://neon.tech/) and sign up for a free account.
2. Click **New Project** and name it `taskflow`.
3. Copy the **Connection Details** connection string (select **Node.js** or **Direct connection**):
   ```
   postgresql://username:password@ep-sample-12345.us-east-2.aws.neon.tech/taskflow?sslmode=require
   ```
4. Paste this into your Render Web Service as the `DATABASE_URL` environment variable.

### Free Option 2: Render PostgreSQL
1. On the Render Dashboard, click **New +** ➔ **PostgreSQL**.
2. Name it `taskflow-db` and select the **Free** tier.
3. Once created, copy the **Internal Database URL** (if backend is on Render) or **External Database URL**.
4. Set it as `DATABASE_URL` in your backend Web Service.

---

## 🧪 Local Testing & Verification

```bash
# 1. Install dependencies
npm install

# 2. Run schema migration check
npm run db:migrate

# 3. Start local development server
npm run dev

# 4. Run automated test suite (Tests health, register, login, profile, and complete task CRUD)
npm run test:api
```

---

## 👩‍💻 Author & Submission
- **Intern**: Sharanya
- **Repository**: [https://github.com/durusojusharanya-ux/crud](https://github.com/durusojusharanya-ux/crud)
- **Internship Task**: Task 3 — Full-Stack Deployment
