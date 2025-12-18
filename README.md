# Financial Ledger API

A RESTful backend API that implements a **double-entry bookkeeping system** for a banking/financial ledger. This project demonstrates **ACID transactions**, **immutable ledger design**, and **balance calculation derived entirely from ledger entries**.

---

## 📌 Project Overview

This API simulates a simple banking ledger system where:

* Accounts can be created
* Money can be deposited
* Money can be transferred between accounts
* Account balances are **never stored directly**
* Balances are always computed from ledger entries

The system follows real-world accounting principles used in financial institutions.

---

## 🛠 Tech Stack

* **Backend**: Node.js, Express.js
* **Database**: PostgreSQL
* **ORM**: Prisma
* **API Testing**: Postman / curl
* **Architecture**: REST API

---

## 🧱 Core Concepts Implemented

### 1️⃣ Double-Entry Bookkeeping

Every financial transaction creates **two ledger entries**:

* **Debit** entry (negative amount)
* **Credit** entry (positive amount)

Example (Transfer ₹300 from A to B):

* Account A → `-300`
* Account B → `+300`

This guarantees that the system is always balanced.

---

### 2️⃣ Immutable Ledger

* Ledger entries are **never updated or deleted**
* Corrections are handled by creating new ledger entries
* This provides full auditability and traceability

---

### 3️⃣ Balance Calculation
Balance = SUM(all ledger entry amounts for an account)
```
---

### 4️⃣ ACID Transactions
* **Atomicity** – All operations succeed or none do
* **Consistency** – Ledger remains valid
* **Isolation** – No dirty reads
* **Durability** – Committed data is permanent

---

### 5️⃣ Overdraft Protection

Before transferring funds, the system:

* Calculates sender’s balance
* Blocks the transaction if balance is insufficient

This prevents negative balances.

---

## 🗄 Database Schema

### Account

* Represents a bank account

Fields:

* `id` (UUID)
* `userId`
* `type`
* `currency`
* `status`
* `createdAt`

---

### Transaction

* Represents a logical financial transaction

Fields:

* `id` (UUID)
* `type` (deposit / transfer)
* `status`
* `amount`
* `createdAt`

---

### LedgerEntry

* Represents individual debit/credit records

Fields:

* `id` (UUID)
* `accountId`
* `transactionId`
* `entryType` (debit / credit)
* `amount`
* `createdAt`

---

## 🚀 API Endpoints

### ➕ Create Account

```
POST /accounts
```

Request:

```json
{
  "userId": "u1",
  "type": "savings",
  "currency": "INR",
  "status": "active"
}
```

---

### 💰 Deposit Money

```
POST /transactions/deposit
```

Request:

```json
{
  "accountId": "ACCOUNT_ID",
  "amount": 1000
}
```

---

### 🔁 Transfer Money

```
POST /transfers
```

Request:

```json
{
  "from": "ACCOUNT_ID_1",
  "to": "ACCOUNT_ID_2",
  "amount": 300
}
```

---

### 📊 Get Account Balance

```
GET /accounts/:id
```

Response:

```json
{
  "balance": "1700"
}
```

---

## 🧪 Testing

All endpoints were tested using:

* `curl`
* Postman

Tests include:

* Valid deposits
* Valid transfers
* Insufficient balance checks
* Balance verification after transactions

---

## 📈 Diagrams

* ER Diagram (Account, Transaction, LedgerEntry)
* System Architecture Diagram (Client → API → Database)

(Attached as images in the repository)

---

## ✅ Assignment Requirements Mapping

| Requirement              | Status |
| ------------------------ | ------ |
| ACID Transactions        | ✅      |
| Double-entry bookkeeping | ✅      |
| Immutable ledger         | ✅      |
| Overdraft prevention     | ✅      |
| REST API                 | ✅      |
| Database-backed          | ✅      |
| Documentation            | ✅      |

---

## 👩‍💻 Author

**Name:** Likhitha

This project was developed as part of a college/internship assignment to demonstrate backend engineering and financial system design concepts.

---

## 📌 Conclusion

This project demonstrates how real-world financial systems are built using:

* Strong data consistency
* Proper accounting principles
* Robust transaction handling

It closely mirrors production-grade ledger systems used in banks and payment platforms.

All critical operations (deposit, transfer) are executed inside **Prisma `$transaction` blocks**, ensuring:


This avoids data inconsistency and reflects real accounting systems.


Account balances are **derived dynamically**:


