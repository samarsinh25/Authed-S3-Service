## Key Subdirectories and Their Roles

| Directory      | Description                                                                                  |
|----------------|----------------------------------------------------------------------------------------------|
| **cmd/**       | Contains your application's entry point (`main.go`).                                         |
| **config/**    | Holds configuration files, such as `.env`.                                                   |
| **internal/**  | Stores private application logic not intended for external import.                           |
| **migrations/**| Contains database migration scripts.                                                         |
| **models/**    | Defines Go structs that map to PostgreSQL tables.                                            |
| **services/**  | Houses the core business logic.                                                              |
| **handlers/**  | Exposes your APIs as HTTP routes.                                                            |
| **middlewares/**| Custom middleware functions for authentication and validation.                              |
| **redis/**     | Contains logic related to Redis queues and background workers.                               |



1. 🧠 WTF IS THIS PROJECT?
This is a secure file upload microservice built in Go (Gin) that:

Accepts file uploads.

Stores them in AWS S3 (or compatible blob storage).

Uses PostgreSQL to track uploaded metadata.

Uses Redis for background jobs like email notifications or queues.

Runs Cron jobs to periodically clean old files.

Is fully dockerized, TDD-ready, and infra-capable.

💥 Basically, this is a backend-only project that simulates how real-world production systems manage files:

Secure access.

Authenticated uploads.

Background tasks.

Periodic cleanups.

Infra automation (Docker + cron + graceful shutdown).

2. ⚙️ HIGH-LEVEL ARCHITECTURE (TOP-DOWN)

| Layer            | Tech Used               | Purpose                                       |
| ---------------- | ----------------------- | --------------------------------------------- |
| API Layer        | Go + Gin                | Exposes REST routes                           |
| Auth Layer       | JWT/Session (TBD)       | Secures file upload                           |
| DB Layer         | PostgreSQL + Migrations | Stores metadata (filename, userID, timestamp) |
| Background Layer | Redis + Worker          | Email queue, task dispatch                    |
| Cron Layer       | Cronjob in Go           | Cleans up old files hourly                    |
| AWS Layer        | S3 SDK                  | Uploads file to cloud bucket                  |
| Docker Layer     | Docker + Docker Compose | Full containerization                         |
| Test Layer       | Testify, mockgen, SQLC  | Unit, mock, and DB tests                      |
| DevOps Layer     | Graceful shutdown       | Clean exit on signal                          |

3. 🔄 HOW COMPONENTS TALK (FLOWCHART)
User (Client)
   ↓
[POST /upload]
   ↓
Gin Handler
   ↓
Validation + Auth
   ↓
Save file metadata in PostgreSQL
   ↓
Upload to S3
   ↓
Queue background task to Redis (e.g. send email)
   ↓
Background Worker (Go routine polling Redis)
   ↓
Sends Email / Notif (placeholder logic)

Meanwhile...

CronJob every hour →
   ↓
Checks DB for expired files
   ↓
Deletes them from S3
   ↓
Removes metadata from DB
