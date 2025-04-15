# MongoDB Replica Set with Docker

This repository sets up a **MongoDB replica set** using Docker Compose. It includes:

- Three MongoDB nodes (Primary, Secondary, Secondary)
- Replica set initialization script
- Mongo Express for UI-based DB access

> 🔗 If you want to load balance reads across secondaries, check out the companion project:  
> 👉 [haproxy-mongodb-balancer](https://github.com/azita-abdollahi/haproxy-mongodb-balancer)

---

## 📦 Requirements

1. **Generate the shared replica set key file** (for internal authentication):

   ```bash
   openssl rand -base64 756 > replica.key
   sudo chown 999:999 replica.key
   chmod 600 replica.key
   ```

2. **Create Docker network** (shared across services):

```bash
docker network create \
  --driver=bridge \
  --subnet=192.168.22.0/24 \
  --ip-range=192.168.22.0/24 \
  --gateway=192.168.22.254 \
  connet
```

3. **Environment variables**: Copy the example and edit as needed:

1. ```bash
   cp .env.example .env
   ```

------

## 🚀 Getting Started

```bash
docker compose up -d
docker compose ps
# down containers with volumes
docker compose down
```

Replica set will auto-init via `mongo_rs_init.sh` once all nodes are healthy.

Mongo Express will be available at http://localhost:8081

------

## 🔧 Services Overview

| Service       | Role        | Port  |
| ------------- | ----------- | ----- |
| mongodb1      | Primary     | 27017 |
| mongodb2      | Secondary   | 27018 |
| mongodb3      | Secondary   | 27019 |
| mongo-express | Web UI      | 8081  |
| mongo-init    | Init script | —     |

------

## 📖 Documentation

Full setup, TLS configuration, replication testing, and troubleshooting guides:

📚 [Wiki Home »](https://github.com/azita-abdollahi/mongodb-replicaset-docker/wiki)

- [Setup Instructions](https://github.com/azita-abdollahi/mongodb-replicaset-docker/wiki/Setup-Instructions)
- [Mongo Express & Replica Set](https://github.com/azita-abdollahi/mongodb-replicaset-docker/wiki/Mongo-Express-&-Replica-Set)
- [TLS Configuration (Recommended)](https://github.com/azita-abdollahi/mongodb-replicaset-docker/wiki/TLS-Configuration)
- [Replica Set Testing & Failover](https://github.com/azita-abdollahi/mongodb-replicaset-docker/wiki/Replica-Set-Testing-&-Failover)
- [Troubleshooting](https://github.com/azita-abdollahi/mongodb-replicaset-docker/wiki/Troubleshooting)

------

## 📌 Notes

- This setup uses keyFile authentication (no TLS by default).
- MongoDB 8.0+ requires TLS for certain modes (e.g., `sendKeyFile`).