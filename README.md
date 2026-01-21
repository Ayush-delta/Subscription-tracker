# Sub-Dub (Subscription Tracker)

A comprehensive backend API for managing subscriptions and sending intelligent, automated email reminders. Built with Node.js, Express, MongoDB, and powered by **Upstash Workflow** for reliable scheduling.

## 🚀 Features

-   **User Authentication**: Secure JWT-based signup and login.
-   **Subscription Management**: CRUD operations for user subscriptions.
-   **Automated Reminders**: Intelligent email notifications sent before subscription renewals.
-   **Smart Scheduling**: Uses **Upstash Workflow** to handle reminders without traditional cron jobs.
-   **Security**: Integrated **Arcjet** for rate limiting and bot protection.
-   **Email Integration**: Automated email dispatch using Nodemailer.

## ⚡ The Workflow (Upstash Integration)

The project leverages **@upstash/workflow** to handle the complexity of scheduling reminders. Instead of checking the database every minute (polling), we schedule "long-running" workflows that sleep until they are needed.

### How it works:
1.  **Trigger**: When a user creates a subscription (`subscription.controller.js`), a new workflow run is triggered immediately.
2.  **Logic Execution**: The workflow endpoint (`/api/v1/workflows/subscription/reminder`) receives the event.
3.  **Sleep & Wake**:
    -   The workflow calculates the renewal date.
    -   It iterates through a defined schedule: **7 days, 5 days, 2 days, and 1 day** before renewal.
    -   For each milestone, the workflow uses `context.sleepUntil` to suspend execution until that exact moment.
4.  **Action**: When the time arrives, the workflow "wakes up", verifies the subscription is still active, and sends the reminder email.

## 🛠️ Setup & Installation

1.  **Clone the repository**
    ```bash
    git clone <repository-url>
    cd Subscription-tracker
    ```

2.  **Install Dependencies**
    ```bash
    npm install
    ```

3.  **Environment Variables**
    Create a `.env.development.local` file in the root directory with the following variables:
    ```env
    PORT=5500
    SERVER_URL="http://localhost:5500"
    NODE_ENV=development

    # Database
    DB_URI=<your_mongodb_uri>

    # JWT Auth
    JWT_SECRET=<your_secret>
    JWT_EXPIRES_IN="1d"

    # Upstash (Workflow)
    QSTASH_URL=http://127.0.0.1:8080
    QSTASH_TOKEN=<your_token>

    # Arcjet (Security)
    ARCJET_KEY=<your_arcjet_key>
    ARCJET_ENV=development

    # Nodemailer
    Email_user=<your_email>
    Email_pass=<your_email_password>
    ```

4.  **Run the local QStash server** (for workflow testing)
    ```bash
    npx @upstash/qstash-cli dev
    ```

5.  **Start the Application**
    ```bash
    npm run dev
    ```

## 📂 Project Structure

-   `controllers/`: Logic for Users, Subscriptions, and Workflows.
-   `routes/`: API route definitions.
-   `models/`: Mongoose schemas.
-   `middlewares/`: Auth and Error handling.
-   `config/`: Configuration for third-party services (Upstash, Arcjet, Nodemailer).
-   `utils/`: Helper functions (Email sending, templates).

## 🔗 API Endpoints

-   `POST /api/v1/auth/sign-up`
-   `POST /api/v1/auth/sign-in`
-   `GET /api/v1/subscriptions`
-   `POST /api/v1/subscriptions`
-   `GET /api/v1/users`
