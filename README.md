// Project: Medusa E-commerce Deployment Setup

// Directory Structure:
// .
// ├── medusa-config.ts
// ├── .env.example
// ├── package.json
// ├── .gitignore
// ├── README.md
// ├── .medusa/
// │   └── server/

// File: medusa-config.ts
import { defineConfig } from "@medusajs/medusa"
import { Modules } from "@medusajs/modules-sdk"

export default defineConfig({
  projectConfig: {
    redisUrl: process.env.REDIS_URL,
    workerMode: process.env.MEDUSA_WORKER_MODE as "server" | "worker" | "shared",
  },
  admin: {
    disable: process.env.DISABLE_MEDUSA_ADMIN === "true",
    backendUrl: process.env.MEDUSA_BACKEND_URL,
  },
  modules: [
    {
      resolve: "@medusajs/medusa/cache-redis",
      options: { redisUrl: process.env.REDIS_URL },
    },
    {
      resolve: "@medusajs/medusa/event-bus-redis",
      options: { redisUrl: process.env.REDIS_URL },
    },
    {
      resolve: "@medusajs/medusa/workflow-engine-redis",
      options: { redis: { url: process.env.REDIS_URL } },
    },
  ],
})

// File: .env.example
COOKIE_SECRET=supersecret123
JWT_SECRET=supersecretjwt123
STORE_CORS=http://localhost:8000
ADMIN_CORS=http://localhost:7000
AUTH_CORS=http://localhost:8000,http://localhost:7000
MEDUSA_BACKEND_URL=http://localhost:9000
MEDUSA_WORKER_MODE=server
DISABLE_MEDUSA_ADMIN=false
PORT=9000
DATABASE_URL=postgresql://username:password@host:port/dbname
REDIS_URL=redis://default:password@host:port

// File: package.json (scripts section)
"scripts": {
  "dev": "medusa develop",
  "build": "medusa build",
  "start": "medusa start",
  "predeploy": "medusa db:migrate"
}

// File: .gitignore
node_modules
.env
.medusa

// File: README.md
# Medusa E-commerce Backend

This is a production-ready Medusa backend setup that supports deployment in server and worker mode using PostgreSQL and Redis.

## Setup Instructions

1. Copy `.env.example` to `.env` and fill in the required values.
2. Run `npm install`
3. Build the app: `npm run build`
4. Deploy `cd .medusa/server && npm install && npm run predeploy && npm run start`

## Deployment Modes
- **Server Mode**: `MEDUSA_WORKER_MODE=server`, `DISABLE_MEDUSA_ADMIN=false`
- **Worker Mode**: `MEDUSA_WORKER_MODE=worker`, `DISABLE_MEDUSA_ADMIN=true`

## Health Check
- Navigate to `/health` to check if the backend is running.

## Admin Dashboard
- Accessible at `/app` after creating an admin user using:
```
npx medusa user -e admin@example.com -p yourpassword
```

---

// Start Commands:
// For server instance:
// cd .medusa/server && npm install && npm run predeploy && npm run start

// For worker instance:
// cd .medusa/server && npm install && npm run start
