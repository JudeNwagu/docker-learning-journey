# Day 7 Commands Reference

This document contains every major Docker Compose command used during Day 7.

The focus of this session was learning how Docker Compose simplifies running multi-container applications, automatically creates networks between services, and works with named volumes for persistent data.

---

# Project Setup

Create a working directory:

```bash
mkdir ~/compose-test
cd ~/compose-test
```

Create the Compose file:

```bash
nano docker-compose.yml
```

---

# Start the Project

Build and start every service defined inside the Compose file.

```bash
docker compose up -d
```

Verify both services are running.

```bash
docker compose ps
```

---

# View Service Logs

Check why a service failed to start.

```bash
docker compose logs db
```

This was used to diagnose the PostgreSQL 18 data path compatibility issue.

---

# Rebuild After Fixing the Compose File

Remove containers, networks, and volumes.

```bash
docker compose down -v
```

Start everything again.

```bash
docker compose up -d
```

Confirm PostgreSQL started successfully.

```bash
docker compose logs db
```

---

# Test Automatic Networking

Open a shell inside the web container.

```bash
docker exec -it compose-test-web-1 bash
```

Update package lists.

```bash
apt-get update
```

Install ping.

```bash
apt-get install -y iputils-ping
```

Ping the database service.

```bash
ping db
```

Confirmed that Docker Compose automatically created a private network and resolved the hostname using only the service name.

Exit the container.

```bash
exit
```

---

# Test Database Persistence

Open PostgreSQL.

```bash
docker exec -it compose-test-db-1 psql -U judoski -d learning_db
```

Create a table.

```sql
CREATE TABLE compose_test (
    id SERIAL PRIMARY KEY,
    note TEXT
);
```

Insert records.

```sql
INSERT INTO compose_test (note) VALUES
('Created via Compose'),
('Docker Compose auto-creates networks'),
('Volumes survive docker compose down'),
('Postgres 18 changed its data path'),
('This is Day 7 of my Docker journey');
```

Verify data.

```sql
SELECT * FROM compose_test;
```

Exit PostgreSQL.

```sql
\q
```

---

# Test Persistence

Stop the project.

```bash
docker compose down
```

Verify the named volume still exists.

```bash
docker volume ls
```

Start the project again.

```bash
docker compose up -d
```

Reconnect to PostgreSQL.

```bash
docker exec -it compose-test-db-1 psql -U judoski -d learning_db
```

Verify data survived.

```sql
SELECT * FROM compose_test;
```

---

# Restart After Closing WSL

If the Docker Engine stops after closing Ubuntu or WSL, restart the project.

```bash
docker compose up -d
```

---

# Second Compose Exercise

Create another project.

```bash
mkdir ~/compose-simple
cd ~/compose-simple
```

Create another Compose file.

```bash
nano docker-compose.yml
```

Start both services.

```bash
docker compose up -d
```

Open a shell.

```bash
docker exec -it compose-simple-site-one-1 bash
```

Install ping.

```bash
apt-get update
apt-get install -y iputils-ping
```

Test networking.

```bash
ping site-two
```

Exit.

```bash
exit
```

Stop the project.

```bash
docker compose down
```

---

# Final Cleanup

Remove containers, networks, and volumes.

```bash
docker compose down -v
```

---

# Key Commands Learned

```bash
docker compose up -d
docker compose ps
docker compose logs
docker compose down
docker compose down -v
docker exec
docker volume ls
ping
```

---

# End of Day Reflection

This was the first day where Docker began feeling like an orchestration platform instead of just a container runtime.

Instead of creating networks, volumes, and containers individually, Docker Compose described the entire application in one YAML file and managed everything automatically.

The biggest lesson wasn't simply learning new commands. It was understanding how Docker Compose ties together networking, storage, and services into a repeatable deployment that can be started or removed with a single command.