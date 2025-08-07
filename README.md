# 📘 Amazon ElastiCache Redis: Installation & Connection Guide

This is a complete guide for **installation and connection to Amazon ElastiCache Redis**, covering both **local testing with Redis CLI** and **cloud-based connection via EC2**.

---

## ✅ 1. What is ElastiCache Redis?

Amazon ElastiCache for Redis is a **fully managed in-memory data store** and cache service. It’s widely used to boost web application performance by reducing latency through fast, in-memory data retrieval.

---

## 🔧 2. ElastiCache Redis Setup in AWS

### Step 1: Create a Redis Cluster

1. Go to **AWS Console → ElastiCache → Redis → Create Cluster**
2. Choose:

   * **Cluster Mode:** Disabled (single node) or Enabled (sharded)
   * **Node type:** `cache.t3.micro` (Free tier eligible)
   * **Replicas:** Optional
   * **VPC:** Select existing VPC
   * **Subnet group:** Default or custom
   * **Security group:** Add inbound port **6379** (from EC2's SG)
3. **Enable Redis Auth Token** (recommended for security)
4. Click **Create**

---

## 🧩 3. Allow Access: Security Group Settings

Modify **Security Group** attached to Redis:

* **Inbound Rule:**

  * Type: Custom TCP
  * Port: 6379
  * Source: EC2 Security Group or specific IP

---

## 💻 4. Connect from EC2 Instance

### Step 1: Launch EC2

* Ensure it’s in the **same VPC and subnet** as Redis
* Use **Amazon Linux 2** or **Ubuntu**

### Step 2: Install Redis CLI

#### For Amazon Linux / Amazon Linux 2:

```bash
sudo yum update -y
sudo yum install curl tar pip gcc jemalloc-devel -y
pip install redis
curl -O http://download.redis.io/redis-stable.tar.gz
tar xzvf redis-stable.tar.gz
cd redis-stable
make
sudo cp src/redis-cli /usr/local/bin/
```

#### For Ubuntu:

```bash
sudo apt update -y
sudo apt install redis-tools -y
```

---

## 🔌 5. Connect to ElastiCache Redis

### Get Redis Endpoint

* Go to **ElastiCache Console → your cluster → Configuration endpoint**

### Standard Redis (No Auth)

```bash
redis-cli -h your-cluster-endpoint.amazonaws.com -p 6379
```

### With AUTH Token:

```bash
redis-cli -h your-cluster-endpoint.amazonaws.com -p 6379 -a your_redis_password
```

### Serverless Redis with TLS:

```bash
redis6-cli --tls -h myredis-wjptpw.serverless.use1.cache.amazonaws.com -p 6379
```

> Use `redis6-cli` for **TLS-based Serverless Redis** connections

---

## 📦 6. Example Redis CLI Commands

```bash
set greeting "Hello from Redis"
get greeting
del greeting

set x hello
get x
```

---

## 🔐 7. Best Practices

* 🔒 Use Redis AUTH in all environments
* 🚫 Do NOT expose Redis to the public internet
* 🔐 Use **Security Groups** and VPC isolation
* 📈 Monitor using **Amazon CloudWatch**

---

## 🧪 8. Optional: Python Redis Client

### Install Redis Client

```bash
pip install redis
```

### Python Example Code

```python
import redis

r = redis.StrictRedis(
    host='your-cluster-endpoint.amazonaws.com',
    port=6379,
    password='your_redis_password',  # Optional if AUTH is not enabled
    decode_responses=True
)

r.set('framework', 'flask')
print(r.get('framework'))
```

---

## 📚 References

* [AWS ElastiCache Docs](https://docs.aws.amazon.com/elasticache/)
* [Redis CLI Reference](https://redis.io/docs/ui/cli/)
* [Redis Python Client](https://pypi.org/project/redis/)

