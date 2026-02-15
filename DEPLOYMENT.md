# Paywatch Deployment Guide

Since Paywatch uses a separate Backend (Node.js) and Frontend (React), the best free deployment strategy is to split them between **Render** (Backend) and **Vercel** (Frontend).

## 1. Deploy Backend (Render)
Render offers a free tier for Web Services that is perfect for Node.js APIs.

1.  Push your code to **GitHub**.
2.  Sign up at [render.com](https://render.com).
3.  Click **"New +"** -> **"Web Service"**.
4.  Connect your GitHub repository.
5.  Configure the service:
    *   **Name**: `paywatch-api` (or similar)
    *   **Root Directory**: Leave empty (defaults to root)
    *   **Environment**: `Node`
    *   **Build Command**: `npm install`
    *   **Start Command**: `npm start`
    *   **Plan**: Free
6.  **Environment Variables** (Scroll down to "Advanced"):
    Add all variables from your `.env.development.local` file, but use production values:
    *   `DB_URI`: Your MongoDB Connection String
    *   `JWT_SECRET`: A strong secret key
    *   `QSTASH_URL`: From Upstash (Workflow)
    *   `QSTASH_TOKEN`: From Upstash (Workflow)
    *   `Email_user`: Your email
    *   `Email_pass`: Your app password
    *   `NODE_ENV`: `production`
    *   **Redis Configuration** (See Step 1.5 below):
        *   `REDIS_HOST`: Your Upstash Redis Endpoint (e.g., `us1-choice-sloth-35212.upstash.io`)
        *   `REDIS_PORT`: `6379` (or the port provided by Upstash)
        *   `REDIS_PASSWORD`: Your Upstash Redis Password

### 1.5 Set up Redis (Upstash)
Since Render doesn't provide a free Redis instance, use **Upstash Redis** (same provider as your Workflows).

1.  Go to [console.upstash.com](https://console.upstash.com).
2.  Click **"Create Database"** under Redis.
3.  Give it a name (e.g., `paywatch-redis`).
4.  Select a region close to your Render region (e.g., `US-East-1`).
5.  Click **"Create"**.
6.  Scroll down to "Connect to your database" -> "Node.js (ioredis)".
7.  Copy the `host`, `port`, and `password` and add them to your Render Environment Variables (Step 1.6).

7.  Click **"Create Web Service"**.
8.  **Copy the Service URL** (e.g., `https://paywatch-api.onrender.com`). You will need this for the frontend.

> **Note:** The free tier on Render spins down after inactivity. The first request might take 50 seconds.

## 2. Deploy Frontend (Vercel)
Vercel is the best platform for React/Vite apps.

1.  Sign up at [vercel.com](https://vercel.com).
2.  Click **"Add New..."** -> **"Project"**.
3.  Import your GitHub repository.
4.  Configure the project:
    *   **Framework Preset**: Vite (should detect automatically)
    *   **Root Directory**: `security-dashboard` (Click strict "Edit" to change this!)
5.  **Environment Variables**:
    *   Key: `VITE_API_URL`
    *   Value: `https://paywatch-api.onrender.com/api/v1` (The URL from Render Step 8 + `/api/v1`)
6.  Click **"Deploy"**.

## 3. Final Checks
1.  Open your Vercel URL.
2.  Try signing up/logging in.
3.  Check the "Security" tab to ensure stats are loading.
