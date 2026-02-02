
# 🟢 Personal Finance App (Mint Clone) — README

## 🚀 Project Overview

This project is a **Mint-like personal finance app** built using **Google Sheets + Google Apps Script**, with a **web dashboard** hosted as an **Apps Script Web App** and embedded in **Google Sites**.
It supports **manual expense entry**, **CSV/PDF import**, and optional **Plaid bank syncing** (paid feature). All user data is stored in Google Sheets, ensuring **user-controlled data**.

---

## 📌 Key Goals

* **Mint-like user experience**
* **Privacy-first**: user data stays in Google Sheets
* **Optional Plaid integration**
* **TOTP login security**
* **Dashboard UI** with charts and budgeting
* **Google Sites documentation & support**
* **User-friendly onboarding**
* **Free + Paid feature model**
* **User-controlled Plaid keys**

---

## 🧱 Architecture

### 🔹 Data Storage

All data is stored in **Google Sheets**:

* `Settings`
* `Transactions`
* `Accounts`
* `Categories`
* `AuditLogs`

### 🔹 Server Logic

**Google Apps Script** acts as the backend:

* Authentication
* TOTP verification
* Plaid API proxy
* CSV/PDF parsing
* CRUD operations

### 🔹 Frontend

**HTML Service (Apps Script)**:

* Dashboard UI
* Charts (Chart.js)
* Tabs: Dashboard, Transactions, Budgets, Net Worth, Upload, Settings, Help

### 🔹 Hosting

* **Apps Script Web App** (secured)
* Embedded into **Google Sites** (public documentation)

---

## 🗂️ Sheet Structure

### Settings Sheet

| Column          | Purpose                        |
| --------------- | ------------------------------ |
| email           | User email                     |
| password_hash   | SHA-256 hash                   |
| totp_secret     | Base32 secret                  |
| plaid_client_id | Encrypted                      |
| plaid_secret    | Encrypted                      |
| plaid_env       | sandbox/development/production |
| created_at      | Timestamp                      |

### Transactions Sheet

| Column         | Purpose              |
| -------------- | -------------------- |
| date           | Transaction date     |
| description    | Description          |
| amount         | Amount               |
| category       | Category             |
| account        | Account name         |
| source         | manual/csv/pdf/plaid |
| transaction_id | Unique ID            |

### Accounts Sheet

| Column      | Purpose                 |
| ----------- | ----------------------- |
| name        | Account name            |
| type        | Checking/Savings/Credit |
| balance     | Current balance         |
| institution | Bank name               |

### Categories Sheet

| Column         | Purpose       |
| -------------- | ------------- |
| name           | Category name |
| monthly_budget | Budget amount |

### AuditLogs Sheet

| Column    | Purpose            |
| --------- | ------------------ |
| timestamp | Timestamp          |
| action    | Action description |

---

## 🔐 Security Model

### Authentication Flow

1. User login with email + password
2. TOTP verification (Google Authenticator compatible)
3. Session token created
4. Auto-logout after inactivity

### Encryption & Hashing

* Passwords hashed using **SHA-256 + salt**
* Plaid keys **encrypted** in Sheet
* No keys exposed to frontend

### TOTP

* RFC 6238 compatible
* Uses `Utilities.computeHmacSha1Signature`

---

## ⚙️ Features

### Free Features

* Manual transaction entry
* CSV upload
* PDF upload (basic)
* Categories & budgets
* Charts & dashboard
* Net worth tracking
* Export data
* Full data ownership

### Paid Features (Optional)

* Plaid bank sync
* Auto transaction import
* Account balances
* Bank metadata

---

## 🔧 Implementation Plan

### MVP Order

1. Manual entry + dashboard
2. Categories + budgets
3. CSV upload
4. Authentication + TOTP
5. Charts
6. Google Sites documentation
7. Plaid integration
8. PDF parsing (optional)

---

## 📌 Apps Script Project Structure

```
Code.gs
Auth.gs
Plaid.gs
Transactions.gs
Upload.gs
Utils.gs
index.html
dashboard.html
settings.html
help.html
```

---

## 🧩 Key Code Snippets (Reference)

### Web App Entry

```javascript
function doGet() {
  return HtmlService.createTemplateFromFile("index")
    .evaluate()
    .setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL);
}
```

### Password Hash

```javascript
function hashPassword(password, salt) {
  return Utilities.computeDigest(
    Utilities.DigestAlgorithm.SHA_256,
    password + salt
  ).map(b => ('0' + (b & 0xFF).toString(16)).slice(-2)).join('');
}
```

### Plaid Request Example

```javascript
function plaidRequest(endpoint, body) {
  const settings = getSettings();
  return UrlFetchApp.fetch(
    `https://${settings.plaid_env}.plaid.com/${endpoint}`,
    {
      method: "post",
      contentType: "application/json",
      payload: JSON.stringify({
        client_id: settings.plaid_client_id,
        secret: settings.plaid_secret,
        ...body
      })
    }
  );
}
```

---

## 📝 Google Sites Pages (Docs + Support)

### Pages

* Home
* How It Works
* How to Use
* How to Get Plaid Keys
* FAQs
* Privacy Policy
* Support

### Support Form

* Google Form (email submissions)
* Link in site footer

---

## 📚 Documentation / Help Content

### Must-Have Guides

* Getting Started
* Adding Expenses
* Uploading Bank Data
* Creating Budgets
* Connecting Plaid
* Data Backup
* Security & Encryption

---

## 🧠 Notes & Reminders

* **All data belongs to the user** (no third-party storage)
* Plaid keys are **optional**
* Plaid integration is **user-provided**
* Dashboard is **Mint-like**
* Google Sites is only for docs & support
* Main app lives in **Apps Script Web App**

---

## ✅ Next Steps

1. Build MVP dashboard + manual entry
2. Add CSV upload
3. Add authentication + TOTP
4. Build Google Sites docs
5. Add Plaid integration

