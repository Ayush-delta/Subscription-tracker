# Paywatch — Subscription Management & Security Platform

Paywatch is a robust SaaS-ready platform designed for tracking user subscriptions, automating email reminders, and monitoring system security in real-time. It features a scalable Node.js backend with advanced security layers and a modern React-based Admin Dashboard for analytics.

![Paywatch Dashboard](./security-dashboard/src/assets/logo2.svg)

---

## 🚀 Features

### 🛡️ Security & Infrastructure
-   **Advanced WAF (Web Application Firewall)**: Protects against SQL Injection, XSS, and command injection attacks.
-   **Bot Detection**: Identifies and blocks malicious crawlers and scrapers.
-   **Rate Limiting**: Prevents DDoS and brute-force attacks.
-   **IP Banning**: Automatically bans suspicious IP addresses.
-   **Unified Activity Logging**: Centralized event stream for users, subscriptions, security, and workflows.

### 📊 Admin Dashboard
-   **Real-Time Analytics**: Live monitoring of total users, active subscriptions, and system load.
-   **Security Intelligence**: Visualizations for attack frequency, top blocked IPs, and recent security events.
-   **Interactive Charts**: Area, Bar, and Pie charts powered by Recharts.
-   **Activity Feed**: Live feed of all system events (User signups, attacks blocked, emails sent).

### 🔄 Subscription Management
-   **CRUD Operations**: Full lifecycle management for user subscriptions.
-   **Automated Reminders**: Intelligent email notifications sent 7, 5, 2, and 1 days before renewal.
-   **Smart Scheduling**: Powered by **Upstash Workflow** for reliable, serverless task scheduling.

---

## 🛠️ Tech Stack

### Backend
-   **Runtime**: Node.js, Express.js
-   **Database**: MongoDB (Mongoose), Aggregation Pipelines.
-   **Security**: Custom WAF, Rate Limiting, Bot Detection.
-   **Automation**: Upstash Workflow (QStash).
-   **Auth**: JWT (JSON Web Tokens), Bcrypt.js.
-   **Email**: Nodemailer.

### Frontend (Admin Dashboard)
-   **Framework**: React 19, Vite.
-   **Styling**: Tailwind CSS.
-   **State/Data**: Axios, React Hooks.
-   **Visualization**: Recharts.
-   **Icons**: Lucide React.
-   **Animations**: Framer Motion.

---

## ⚙️ Installation & Setup

### 1. Prerequisites
-   Node.js (v18+)
-   MongoDB Atlas URI or local instance
-   Upstash Account (for Workflow/QStash)

### 2. Backend Setup
1.  **Clone the repository**
    ```bash
    git clone <repository-url>
    cd Subscription-tracker
    ```

2.  **Install Backend Dependencies**
    ```bash
    npm install
    ```

3.  **Configure Environment Variables**
    Create a `.env.development.local` file in the root directory:
    ```env
    # Server Configuration
    PORT=5500
    SERVER_URL="http://localhost:5500"
    NODE_ENV=development

    # Database
    DB_URI=<your_mongodb_connection_string>

    # Authentication
    JWT_SECRET=<your_jwt_secret>
    JWT_EXPIRES_IN="1d"

    # Upstash (Workflow Automation)
    QSTASH_URL=<your_qstash_url>
    QSTASH_TOKEN=<your_qstash_token>

    # Redis (Local or Cloud)
    REDIS_HOST="127.0.0.1"
    REDIS_PORT=6379
    # REDIS_PASSWORD=<optional>

    # Email Service (Nodemailer)
    Email_user=<your_email_address>
    Email_pass=<your_email_app_password>
    ```

4.  **Start Background Services (Redis)**
    Since the application uses Redis for rate limiting, you need to have it running. The easiest way is using Docker:
    ```bash
    docker-compose up -d
    ```

5.  **Start the Backend**
    ```bash
    npm run dev
    ```
    The API will run at `http://localhost:5500`.

### 3. Frontend Dashboard Setup
1.  **Navigate to the dashboard directory**
    ```bash
    cd security-dashboard
    ```

2.  **Install Frontend Dependencies**
    ```bash
    npm install
    ```

3.  **Start the Dashboard**
    ```bash
    npm run dev
    ```
    The dashboard will launch at `http://localhost:5173`.

---

## 📂 Project Structure
This project follows a **Feature-Based Architecture**, ensuring scalability and modularity.

```
Paywatch/
├── src/
│   ├── auth/               # Authentication Feature
│   │   ├── auth.controller.js
│   │   ├── auth.routes.js
│   │   ├── auth.service.js
│   │   └── auth.schema.js
│   ├── subscription/       # Subscription Feature
│   │   ├── subscription.controller.js
│   │   ├── subscription.routes.js
│   │   └── subscription.model.js
│   ├── security/           # Security Feature
│   │   ├── security.middleware.js
│   │   ├── waf.js
│   │   └── rateLimiter.js
│   ├── analytics/          # Analytics Feature
│   │   └── analytics.controller.js
│   ├── workflows/          # Automation Workflows
│   │   └── reminder.workflow.js
│   ├── shared/             # Shared Utilities
│   │   ├── database/
│   │   ├── utils/
│   │   └── middleware/     # Global Middleware (Error handling, etc.)
│   └── app.js              # Express Entry Point
├── security-dashboard/     # React Admin Dashboard
└── package.json            # Backend Dependencies
```

---

## 🔗 Key API Endpoints

### 🔐 Auth & Users
-   `POST /api/v1/auth/sign-up` - Register new user
-   `POST /api/v1/auth/sign-in` - Login
-   `GET /api/v1/users` - Get all users

### 💳 Subscriptions
-   `GET /api/v1/subscriptions` - List all subscriptions
-   `POST /api/v1/subscriptions` - Create subscription (Triggers workflow)
-   `GET /api/v1/subscriptions/user/:id` - Get user specific subscriptions

### 🛡️ Admin & Security
-   `GET /api/v1/admin/users/stats` - User growth stats
-   `GET /api/v1/admin/security/stats` - Security block stats
-   `GET /api/v1/admin/activity` - Global activity feed
-   `GET /api/v1/security/logs` - Recent security logs
-   `POST /api/v1/workflows/subscription/reminder` - Trigger reminder workflow

---

## 🛡️ Security Features Testing
To test the security features, try running these commands:

**Test Bot Detection:**
```bash
curl -H "User-Agent: python-requests" http://localhost:5500/api/v1/users
```

**Test WAF (SQL Injection):**
```bash
curl "http://localhost:5500/api/v1/users?id=1 UNION SELECT * FROM users"
```

Check the **Security Dashboard** to see these attacks blocked in real-time!
