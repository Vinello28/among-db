# 📚 Among-DB

> A MongoDB containerized learning environment for the **New Generation Databases** course at UnivPM. Designed to practice NoSQL concepts, aggregation pipelines, and database management in a isolated Docker environment.

## 🎯 Overview

This repository provides a fully containerized MongoDB instance pre-configured for coursework exercises. It includes sample datasets and is ready for immediate use without complex setup procedures.

**Perfect for:**
- Learning NoSQL database concepts
- Practicing MongoDB queries and aggregations
- Understanding document-oriented data modeling
- Running reproducible database exercises

---

## 📋 Prerequisites

- **Docker** (v20.10+) - [Install Docker](https://docs.docker.com/get-docker/)
- **Docker Compose** (v1.29+) - Usually included with Docker Desktop

**Verify your installation:**
```bash
docker --version
docker-compose --version
```

---

## 🚀 Quick Start

### 1. Start the Database

```bash
cd docker
docker-compose up -d
```

The MongoDB instance will be available at `localhost:27017`

### 2. Verify Connection

```bash
docker exec -it mongodb_among mongosh -u admin -p password
```

You should see the MongoDB shell prompt `>`. Type `exit` to quit.

### 3. Stop the Database

```bash
docker-compose down
```

---

## 📁 Project Structure

```
among-db/
├── README.md                    # This file
├── docker/
│   ├── docker-compose.yml       # MongoDB container configuration
│   └── data/                    # MongoDB persistent data volume
├── docs/
│   ├── TestoQuery.pdf           # Exercise instructions & query reference
│   └── restaurants.json         # Sample dataset (restaurants collection)
└── scripts/                     # (Future: automation scripts)
```

---

## 🗄️ Database Configuration

| Parameter | Value |
|-----------|-------|
| **Host** | `localhost` |
| **Port** | `27017` |
| **Username** | `admin` |
| **Password** | `password` |
| **Container Name** | `mongodb_among` |

### Connection Strings

**MongoDB Shell (mongosh):**
```bash
mongosh mongodb://admin:password@localhost:27017
```

**Mongo Compass URI:**
```
mongodb://admin:password@localhost:27017
```

**Node.js (Mongoose):**
```javascript
const uri = 'mongodb://admin:password@localhost:27017/exercise?authSource=admin';
```

**Python (PyMongo):**
```python
from pymongo import MongoClient
client = MongoClient('mongodb://admin:password@localhost:27017/?authSource=admin')
```

---

## 💾 Loading Sample Data

### Option 1: Using mongosh

```bash
docker exec -it mongodb_among mongosh -u admin -p password

# Inside the shell:
use exercise
db.restaurants.drop()
load('/data/db/../../../docs/restaurants.json')  # Adjust path as needed
```

### Option 2: Using mongoimport

```bash
docker exec mongodb_among mongoimport \
  --username admin \
  --password password \
  --authenticationDatabase admin \
  --db exercise \
  --collection restaurants \
  --file /data/db/../../../docs/restaurants.json \
  --jsonArray
```

---

## 🔍 Usage Examples

### Connect and Explore

```bash
# Access the MongoDB shell
docker exec -it mongodb_among mongosh -u admin -p password

# List databases
show databases

# Switch to exercise database
use exercise

# List collections
show collections

# Query restaurants
db.restaurants.find().limit(5).pretty()

# Count documents
db.restaurants.countDocuments()
```

### Common Operations

**Find all restaurants:**
```javascript
db.restaurants.find({})
```

**Find by cuisine type:**
```javascript
db.restaurants.find({ cuisine: "Italian" })
```

**Find with aggregation:**
```javascript
db.restaurants.aggregate([
  { $group: { _id: "$cuisine", count: { $sum: 1 } } },
  { $sort: { count: -1 } }
])
```

---

## 🛠️ Development Tips

### View Logs

```bash
docker-compose logs mongodb_among
docker-compose logs -f mongodb_among  # Follow logs
```

### Rebuild the Container

```bash
docker-compose down
docker-compose up -d --build
```

### Access Data Directly

```bash
# View persisted data
ls -la docker/data/
```

### Reset the Database

```bash
# Stop and remove volume (⚠️ deletes all data)
docker-compose down -v

# Restart fresh
docker-compose up -d
```

### Performance Check

```bash
docker exec -it mongodb_among mongosh -u admin -p password
db.adminCommand("ping")
```

---

## 📖 Course Materials

- **Query Reference:** See `docs/TestoQuery.pdf` for exercise instructions
- **Sample Data:** `docs/restaurants.json` contains the restaurants dataset

---

## ⚙️ Troubleshooting

**Port 27017 already in use?**
```bash
# Change the port in docker-compose.yml:
ports:
  - "27018:27017"  # Use 27018 instead
```

**Cannot connect to MongoDB?**
- Verify the container is running: `docker ps`
- Check logs: `docker-compose logs mongodb_among`
- Ensure port 27017 is not blocked by firewall

**Connection refused?**
- Wait a few seconds for MongoDB to fully start
- Try: `docker-compose restart mongodb_among`

---

## 📝 Notes

- Default credentials are set for **learning purposes only**. Change them for production use.
- Data persists in `docker/data/` even after stopping the container.
- MongoDB data is **not** included in version control (`.gitignore` should exclude `docker/data/`).

---

## 🎓 Learning Objectives

This setup supports learning:
- ✅ MongoDB CRUD operations
- ✅ Document data modeling
- ✅ Aggregation pipelines
- ✅ Indexing strategies
- ✅ NoSQL vs SQL concepts
- ✅ Container orchestration basics

---

## 📧 Support

For course-related questions, refer to the course materials or contact your instructor.

---

**Last Updated:** April 2026 | **Course:** New Generation Databases | **University:** UnivPM