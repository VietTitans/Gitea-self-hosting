# Gitea + MySQL with Docker Compose

This project sets up a self-hosted [Gitea](https://gitea.io/) instance with a MySQL database using Docker Compose.  
It uses a **hybrid volume strategy** (bind mount for Gitea, named volume for MySQL) and stores credentials in an `.env` file.  
MySQL is monitored with a healthcheck to ensure Gitea only starts when the database is ready.

---

## 🚀 Getting Started

### 1. Clone this repository
git clone https://github.com/yourusername/gitea-docker-compose.git
cd gitea-docker-compose
2. Create an .env file
Copy the example and update values as needed:

cp .env.example .env

.env file:
# User IDs (replace with your actual values from `id -u` and `id -g`)
USER_UID=1000
USER_GID=100

# MySQL root credentials
MYSQL_ROOT_PASSWORD=supersecretroot

# Gitea database credentials
GITEA_DB_USER=gitea
GITEA_DB_PASSWORD=supersecretpassword
GITEA_DB_NAME=gitea
⚠️ Don’t commit .env to git. Add it to .gitignore.

3. Start the stack
docker-compose up -d

4. Access Gitea
Web UI: http://localhost:3000

Git via SSH: ssh -p 222 git@localhost

🗂️ Data Persistence
Gitea data/configs/repos → stored in ./gitea (bind mount, easy to inspect & back up).

MySQL database → stored in Docker named volume gitea-mysql (managed by Docker).

Check your volumes:
docker volume ls
🩺 Healthcheck
MySQL is monitored with a healthcheck:

healthcheck:
  test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
  interval: 10s
  timeout: 5s
  retries: 5
Runs every 10s.

Requires 5 failures before marking MySQL as unhealthy.

Gitea waits until MySQL is healthy before starting.
