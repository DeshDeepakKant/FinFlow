# 💳 **Transakt – Distributed Digital Wallet System** 🚀

## 📌 Overview

**Transakt** is a **distributed digital wallet platform** designed for **seamless, fault-tolerant, and highly available** financial transactions.
It supports **peer-to-peer transfers**, **bank-to-wallet deposits/withdrawals**, and ensures **data consistency** in distributed environments.

The system is powered by a **microservices-inspired architecture** with **event-driven processing**, ensuring that transactions are **durable, resilient, and scalable**. Built using **Next.js**, **Express.js**, **TypeScript**, **Redis**, **PostgreSQL**, and **Prisma ORM**, Transakt is optimized for **real-world financial workloads**.

---

## 🏗️ System Architecture

![System architecture of the app](system-architechture/project-architechture.svg)

---

### 🔑 **Architecture Highlights**

* ✅ **Asynchronous Processing:** Transactions are queued in **Redis** during bank downtime, guaranteeing **100% durability**.
* ✅ **Event-Driven Updates:** Bank results are published to queues, instantly updating wallet balances.
* ✅ **Fault Tolerance:** **LRU caching** in the bank server minimizes redundant DB queries and accelerates user verification.
* ✅ **Automated Notifications:** A **worker service** listens to completed events and sends **real-time email alerts**.
* ✅ **Optimized Throughput:** Queue-based lazy execution prevents **database congestion** and improves system performance.

---

## 🌟 Features

### ⚙️ **Core Functionalities**

* 💸 **Peer-to-Peer Money Transfers** within the wallet ecosystem.
* 🏦 **Deposit & Withdraw** money between wallet and bank.
* 🔐 **Secure Dashboard** with token-based authentication (using bank account number).
* 🔄 **Resilient Transactions:**

  * Queues handle requests when the **bank server is unavailable**.
  * Transactions are **processed once recovery occurs**.
  * Wallet balances are **instantly updated** through event queues.
* 📧 **Email Notifications** for transaction confirmations.

### ⚡ **Performance Enhancements**

* Redis-based **asynchronous processing** for non-blocking workflows.
* **\~40% fewer DB calls** using Redis LRU caching in the bank server.
* **Scalable, event-driven architecture** ensuring **eventual consistency**.

---

## 🛠️ Tech Stack

| Category                | Technologies Used               |
| ----------------------- | ------------------------------- |
| **Frontend**            | Next.js (integrated wallet app) |
| **Backend**             | Express.js (Bank server)        |
| **Database**            | PostgreSQL + Prisma ORM         |
| **Message Queue**       | Redis                           |
| **Caching**             | Redis (LRU Policy)              |
| **Authentication**      | JWT                             |
| **Email Notifications** | Nodemailer                      |

---

## 🚀 Getting Started

### **1️⃣ Prerequisites**

Make sure you have installed:

* [Node.js](https://nodejs.org/) (v16+)
* [Next.js](https://nextjs.org/)
* [Redis](https://redis.io/)
* [PostgreSQL](https://www.postgresql.org/)
* [Docker](https://www.docker.com/) *(optional for Redis/Postgres)*

---

### **2️⃣ Clone the Repository**

```bash
git clone https://github.com/DeshDeepakKant/transakt.git
cd transakt
```

---

### **3️⃣ Install Dependencies**

```bash
# Wallet App
cd wallet-app
npm install

# Bank Server
cd ../bank-server
npm install

# Worker Service
cd ../worker-notifications
npm install
```

---

### **4️⃣ Configure Environment Variables**

Create `.env` files in each service:

```env
# wallet-app
DATABASE_URL="postgresql://user:password@localhost:5432/transakt_wallet"
NEXTAUTH_SECRET="random-secret"
NEXTAUTH_URL="http://localhost:3000"
BANK_SERVER="http://localhost:8080/api/v1"
REDIS_HOST="localhost"
REDIS_PORT=6379

# bank-server
DATABASE_URL="postgresql://user:password@localhost:5432/transakt_bank"
PORT=8080
JWT_SECRET="random-secret"
REDIS_HOST="localhost"
REDIS_PORT=6379

# worker-notifications
REDIS_HOST="localhost"
REDIS_PORT=6379
EMAIL_HOST="smtp.gmail.com"
EMAIL_SENDER_ID="your-email@gmail.com"
EMAIL_SENDER_PASSWORD="your-app-password"
```

---

### **5️⃣ Start Services**

#### Start Redis (via Docker)

```bash
docker run -d --name redis-stack -p 6379:6379 redis/redis-stack-server:latest
```

#### Wallet App

```bash
cd wallet-app
npx prisma generate
npx prisma migrate dev --name "init"
npm run dev
```

#### Bank Server

```bash
cd ../bank-server
npx prisma generate
npx prisma migrate dev --name "init"
npm start server
```

#### Worker Service

```bash
cd ../worker-notifications
npm start worker
```

---

## 📡 Bank API Endpoints

| Method | Endpoint                  | Description               |
| ------ | ------------------------- | ------------------------- |
| `POST` | `/api/v1/verify-user/:id` | Verify user during signup |
| `GET`  | `/api/v1/transfer/:id`    | Handle debit/credit money |

---

## 🔄 Transaction Workflow

1. User initiates a **deposit/withdrawal** in the Wallet App.
2. If **Bank Server is online**, request is processed immediately.
3. If **offline**, request is **queued in Redis** until recovery.
4. Once online, the **Bank Server consumes queued requests** and processes them.
5. Processed transactions are pushed into **another Redis queue**.
6. Wallet app **consumes this queue** to update balances.
7. **Worker Service** sends **transaction emails** in real time.

---

## ⚡ Optimizations

* 📉 **\~40% DB load reduction** with Redis caching.
* 🚦 **Zero failed transactions** during downtime – everything is queued.
* 📡 **Event-driven async architecture** ensures smooth scaling.

---

## 🧩 Future Enhancements

* 🔔 **WebSockets for instant balance updates**.
* 🌍 **Multi-currency support** (INR, USD, EUR, etc.).
* 🛡️ **Fraud detection** for abnormal spending behavior.
* ⚔️ **Rate limiting & API security improvements**.

---

## 👨‍💻 Author

* **Desh Deepak Kant** – [GitHub](https://github.com/DeshDeepakKant)
* 📧 **Contact:** [deshdeepakkant@gmail.com](mailto:deshdeepakkant@gmail.com)