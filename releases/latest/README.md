<div align="center">

# 🚀 Bulk Repository Creator

**Upload multiple folders → Create GitHub repositories → Push code automatically**

[![CI](https://github.com/ankhangbc2021-oss/App-create-repo/actions/workflows/ci.yml/badge.svg)](https://github.com/ankhangbc2021-oss/App_create_repositories/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Node.js](https://img.shields.io/badge/Node.js-20+-green.svg)](https://nodejs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-blue.svg)](https://typescriptlang.org/)
[![React](https://img.shields.io/badge/React-19-61DAFB.svg)](https://react.dev/)

<br />

*A desktop application by Tang Manh Khang that allows you to upload `.zip` archives containing multiple folders and automatically creates a GitHub repository for each one — complete with git initialization, file commits, and code pushing.*

</div>

---

## 📸 Screenshots

<!-- Add your screenshots here -->
<!-- ![Dashboard](docs/screenshots/dashboard.png) -->
<!-- ![Upload](docs/screenshots/upload.png) -->
<!-- ![Queue](docs/screenshots/queue.png) -->

---

## ✨ Features

### Core Features
- ✅ **Bulk Repository Creation** — Upload a `.zip` archive containing multiple folders, each becomes a GitHub repository
- ✅ **Automatic Code Pushing** — Server automatically extracts, git inits, commits, and pushes all files
- ✅ **Real-Time Progress** — Live progress tracking via Server-Sent Events (SSE)
- ✅ **Queue Management** — Concurrent processing with configurable limits
- ✅ **Retry System** — Automatic retry for failed operations with exponential backoff
- ✅ **Rollback Support** — Undo repository creation on failure

### Authentication
- ✅ **GitHub OAuth** — Secure login via GitHub OAuth2 flow
- ✅ **Personal Access Token** — Direct PAT authentication support
- ✅ **Organization Support** — Create repos under personal account or organizations

### Upload System
- ✅ **Cloud-Ready Architecture** — Securely extract and process files on remote servers
- ✅ **Drag & Drop** — Drag `.zip` archives directly into the browser
- ✅ **Automatic Cleanup** — Ephemeral extraction ensures server disk space is never wasted
- ✅ **Project Detection** — Auto-detect project type (React, Node, Python, etc.)
- ✅ **Auto .gitignore** — Generate appropriate .gitignore for detected project types

### User Interface
- ✅ **Modern SaaS Dashboard** — Premium UI inspired by GitHub, Vercel, and Linear
- ✅ **Dark/Light Theme** — System-aware theme with manual toggle
- ✅ **Multi-Language** — English and Vietnamese (i18next)
- ✅ **Responsive Design** — Works on desktop, tablet, and mobile
- ✅ **Live Log Viewer** — Real-time log streaming with search and filters
- ✅ **Statistics Dashboard** — Charts and metrics for upload sessions

### Enterprise Features
- ✅ **History Tracking** — SQLite database for operation history
- ✅ **Export Results** — Download results as CSV/JSON
- ✅ **Rate Limiting** — Configurable API rate limiting
- ✅ **Security Headers** — Helmet, CORS, CSP configuration
- ✅ **Health Checks** — API health endpoints for monitoring

---

## 🛠️ Technology Stack

| Layer | Technology |
|-------|-----------|
| **Frontend** | React 19, TypeScript, Vite |
| **UI Framework** | TailwindCSS, shadcn/ui, Radix UI |
| **State Management** | Zustand |
| **Data Fetching** | TanStack Query |
| **Forms** | React Hook Form, Zod |
| **Charts** | Recharts |
| **Animations** | Framer Motion |
| **i18n** | i18next, react-i18next |
| **Backend** | Node.js 20+, Express.js, TypeScript |
| **Git Operations** | simple-git, Git CLI |
| **File Upload** | Multer |
| **Database** | SQLite (Prisma ORM) |
| **Queue** | p-limit |
| **Logging** | Winston |
| **Security** | Helmet, CORS, express-rate-limit |
| **Realtime** | Server-Sent Events (SSE) |
| **DevOps** | Docker, Nginx, PM2, GitHub Actions |

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Client (React SPA)                   │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌────────┐   │
│  │  Pages   │  │Components│  │  Hooks   │  │ Store  │   │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └───┬────┘   │
│       └─────────────┴─────────────┴────────────┘        │
│                        │ Axios / SSE                    │
└────────────────────────┼────────────────────────────────┘
                         │
                    ┌────┴────┐
                    │  Nginx  │ (Reverse Proxy)
                    └────┬────┘
                         │
┌────────────────────────┼────────────────────────────────┐
│                  Server (Express API)                   │
│  ┌──────────┐  ┌───────────┐  ┌──────────┐  ┌────────┐  │
│  │  Routes  │→ │Controllers│→ │ Services │→ │  Utils │  │
│  └──────────┘  └───────────┘  └───┬──────┘  └────────┘  │
│                                   │                     │
│  ┌──────────┐  ┌──────────┐  ┌────┴─────┐               │
│  │  Queue   │  │  Events  │  │ GitHub   │               │
│  │ Service  │  │  (SSE)   │  │ Service  │               │
│  └──────────┘  └──────────┘  └─────┬────┘               │
│                                    │                    │
│                              ┌─────┴─────┐              │
│                              │Git Service│              │
│                              └───────────┘              │
└─────────────────────────────────────────────────────────┘
                         │
                    ┌────┴────┐
                    │ SQLite  │ (Prisma ORM)
                    └─────────┘
```

---

## 📋 Prerequisites

| Requirement | Version | Required |
|-------------|---------|----------|
| **Node.js** | ≥ 20.0.0 | ✅ |
| **npm** | ≥ 9.0.0 | ✅ |
| **Git** | ≥ 2.30.0 | ✅ |
| **Docker** | ≥ 24.0 | Optional |
| **Docker Compose** | ≥ 2.20 | Optional |

---

## 🚀 Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/ankhangbc2021-oss/App-create-repo.git
cd App-create-repo
```

### 2. Run Setup Script

**Linux/macOS:**
```bash
chmod +x scripts/setup.sh
./scripts/setup.sh
```

**Windows (PowerShell):**
```powershell
.\scripts\setup.ps1
```

### 3. Manual Setup (Alternative)

```bash
# Install dependencies
npm ci
cd client && npm ci && cd ..
cd server && npm ci && cd ..

# Copy environment template to create your .env configuration
# .env.example is a template without real secrets
# .env contains the real configuration and MUST never be committed to Git
cp .env.example .env

# Generate Prisma client
cd server && npx prisma generate && npx prisma db push && cd ..
```

### 4. Configure Environment

Edit `.env` with your GitHub OAuth credentials (see [GitHub OAuth Setup](#-github-oauth-setup) below).
**Note:** `.env.example` is only a template and should never contain real API keys. New developers should always copy `.env.example` to `.env` before running the project.

### 5. Start Development Server

```bash
npm run dev
```

Open [http://localhost:5173](http://localhost:5173) in your browser.

---

## 🔐 Environment Variables

| Variable | Description | Default | Required |
|----------|-------------|---------|----------|
| `PORT` | Server port | `3001` | No |
| `NODE_ENV` | Environment mode | `development` | No |
| `CLIENT_URL` | Frontend URL | `http://localhost:5173` | Yes |
| `GITHUB_CLIENT_ID` | GitHub OAuth App Client ID | — | Yes |
| `GITHUB_CLIENT_SECRET` | GitHub OAuth App Client Secret | — | Yes |
| `GITHUB_CALLBACK_URL` | OAuth callback URL | `http://localhost:3001/api/v1/auth/github/callback` | Yes |
| `SESSION_SECRET` | Session encryption secret | — | Yes |
| `MAX_FILE_SIZE` | Maximum upload size (bytes) | `2147483648` (2GB) | No |
| `MAX_FOLDERS` | Maximum folders per upload | `100` | No |
| `MAX_FILES` | Maximum files per upload | `100000` | No |
| `TEMP_DIR` | Temporary upload directory for ZIP extraction | `./uploads/temp` | No |
| `UPLOAD_DIR` | Upload directory | `./uploads` | No |
| `MAX_CONCURRENT_JOBS` | Concurrent repository creation jobs | `5` | No |
| `MAX_RETRY_COUNT` | Maximum retry attempts | `3` | No |
| `QUEUE_TIMEOUT` | Queue job timeout (ms) | `300000` | No |
| `LOG_LEVEL` | Logging level | `info` | No |
| `LOG_DIR` | Log directory | `./logs` | No |
| `DATABASE_URL` | SQLite database URL | `file:./data/app.db` | No |
| `RATE_LIMIT_WINDOW_MS` | Rate limit window (ms) | `900000` | No |
| `RATE_LIMIT_MAX_REQUESTS` | Max requests per window | `100` | No |
| `CORS_ORIGINS` | Allowed CORS origins | `http://localhost:5173` | No |
| `ROLLBACK_ENABLED` | Enable rollback on failure | `true` | No |
| `AUTO_CLEANUP` | Auto cleanup temp files | `true` | No |
| `CLEANUP_INTERVAL_MS` | Cleanup interval (ms) | `3600000` | No |
| `GIT_LFS_THRESHOLD` | Git LFS file size threshold (bytes) | `52428800` (50MB) | No |

---

## 🔑 GitHub OAuth Setup

### Step 1: Create a GitHub OAuth App

1. Go to [GitHub Settings → Developer settings → OAuth Apps](https://github.com/settings/developers)
2. Click **"New OAuth App"**
3. Fill in the form:

| Field | Development Value | Production Value |
|-------|------------------|-----------------|
| **Application name** | `Bulk Repository Creator (Dev)` | `Bulk Repository Creator` |
| **Homepage URL** | `http://localhost:5173` | `https://your-domain.com` |
| **Authorization callback URL** | `http://localhost:3001/api/v1/auth/github/callback` | `https://your-domain.com/api/v1/auth/github/callback` |

4. Click **"Register application"**
5. Copy the **Client ID**
6. Click **"Generate a new client secret"** and copy the **Client Secret**

### Step 2: Configure Environment

```env
GITHUB_CLIENT_ID=your_client_id_here
GITHUB_CLIENT_SECRET=your_client_secret_here
GITHUB_CALLBACK_URL=http://localhost:3001/api/v1/auth/github/callback
```

### Required OAuth Scopes

The OAuth app requires the following scopes:
- `repo` — Full control of private repositories
- `read:org` — Read organization membership
- `read:user` — Read user profile data
- `user:email` — Access user email addresses

---

## 🔑 GitHub Personal Access Token (PAT) Setup

### Step 1: Create a Fine-Grained PAT

1. Go to [GitHub Settings → Developer settings → Personal access tokens → Fine-grained tokens](https://github.com/settings/tokens?type=beta)
2. Click **"Generate new token"**
3. Configure:
   - **Token name**: `Bulk Repo Creator`
   - **Expiration**: Choose appropriate duration
   - **Repository access**: All repositories
   - **Permissions**:
     - Repository permissions:
       - **Administration**: Read and write
       - **Contents**: Read and write
       - **Metadata**: Read-only
4. Click **"Generate token"** and copy the token

### Step 2: Use in Application

Enter the PAT in the application's authentication dialog instead of using OAuth.

---



## 📡 API Documentation

For the complete API reference, see [docs/api/README.md](docs/api/README.md).

### Quick Reference

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/v1/health` | Health check |
| `GET` | `/api/v1/auth/github` | GitHub OAuth login |
| `GET` | `/api/v1/auth/github/callback` | OAuth callback |
| `POST` | `/api/v1/auth/token` | PAT authentication |
| `GET` | `/api/v1/auth/me` | Current user info |
| `POST` | `/api/v1/auth/logout` | Logout |
| `GET` | `/api/v1/github/user` | GitHub user profile |
| `GET` | `/api/v1/github/orgs` | User organizations |
| `POST` | `/api/v1/upload/folders` | Upload folders |
| `POST` | `/api/v1/queue/start` | Start queue processing |
| `POST` | `/api/v1/queue/pause` | Pause queue |
| `POST` | `/api/v1/queue/resume` | Resume queue |
| `DELETE` | `/api/v1/queue/cancel` | Cancel all jobs |
| `POST` | `/api/v1/queue/retry/:id` | Retry failed job |
| `GET` | `/api/v1/events` | SSE event stream |
| `GET` | `/api/v1/logs` | Get logs |
| `GET` | `/api/v1/history` | Operation history |
| `GET` | `/api/v1/settings` | Get settings |
| `PUT` | `/api/v1/settings` | Update settings |
| `GET` | `/api/v1/system/info` | System information |

---

## 🔧 Troubleshooting

### Common Issues

<details>
<summary><strong>Port 3001 is already in use</strong></summary>

```bash
# Find the process using port 3001
# Linux/macOS
lsof -i :3001
kill -9 <PID>

# Windows
netstat -ano | findstr :3001
taskkill /PID <PID> /F
```
</details>

<details>
<summary><strong>GitHub OAuth callback error</strong></summary>

1. Verify your `GITHUB_CALLBACK_URL` matches exactly what's configured in GitHub OAuth App settings
2. Ensure `CLIENT_URL` is correct in `.env`
3. Check that the server is running on the expected port
</details>

<details>
<summary><strong>Prisma database errors</strong></summary>

```bash
# Reset the database
cd server
npx prisma db push --force-reset

# Regenerate Prisma client
npx prisma generate
```
</details>

<details>
<summary><strong>Git push failures</strong></summary>

1. Ensure Git is installed and accessible: `git --version`
2. Check that the GitHub token has `repo` scope
3. Verify network connectivity to GitHub
4. Check if the repository name already exists
</details>

<details>
<summary><strong>Large file upload failures</strong></summary>

1. Increase `MAX_FILE_SIZE` in `.env`
2. If using Nginx, ensure `client_max_body_size` is set appropriately
3. Consider enabling Git LFS for large files (adjust `GIT_LFS_THRESHOLD`)
</details>

<details>
<summary><strong>Docker build failures</strong></summary>

```bash
# Clean Docker cache
docker system prune -a

# Rebuild without cache
docker compose build --no-cache

# Check disk space
docker system df
```
</details>

---

## ❓ FAQ

**Q: How many repositories can I create at once?**
A: The application supports creating hundreds of repositories in a single session. The limit is configurable via `MAX_FOLDERS` (default: 100).

**Q: Does it work with GitHub Enterprise?**
A: Yes. Set `GITHUB_API_URL` and `GITHUB_BASE_URL` in your `.env` file to point to your GitHub Enterprise instance.

**Q: Can I create repositories under an organization?**
A: Yes. After authentication, select the target organization from the dropdown in the upload settings.

**Q: What happens if the upload fails midway?**
A: Failed operations are automatically retried (configurable via `MAX_RETRY_COUNT`). If rollback is enabled, partially created repositories will be cleaned up.

**Q: Does it support private repositories?**
A: Yes. You can configure the default visibility (public/private) in settings, or set it per-upload.

**Q: How does it handle large files?**
A: Files exceeding `GIT_LFS_THRESHOLD` (default: 50MB) are tracked with Git LFS. The maximum total upload size is configurable via `MAX_FILE_SIZE`.

**Q: Can I use this without Docker?**
A: Absolutely. You can run the application directly with Node.js using `npm run dev` or `npm run build && npm start`.

---

## 🤝 Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

<div align="center">

**Made with ❤️ for the GitHub community by Tang Manh Khang**

[Report Bug](https://github.com/ankhangbc2021-oss/App-create-repo/issues) · [Request Feature](https://github.com/ankhangbc2021-oss/App-create-repo/issues) · [Discussions](https://github.com/ankhangbc2021-oss/App-create-repo/discussions)

</div>   