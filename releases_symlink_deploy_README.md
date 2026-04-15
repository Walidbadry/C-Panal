# 🚀 Deploy Strategy: Releases + Symlink (Zero Downtime)

## 📌 Overview

This project uses a production-grade deployment strategy inspired by real-world DevOps systems.

Instead of overwriting the same codebase, every deployment creates a **new release**, and the active version is switched using a **symbolic link (symlink)** called `current`.

This allows **zero downtime deployments**, fast rollback, and clean architecture.

---

## 📁 Project Structure

app/
├── releases/
│   ├── 20260415_1200/
│   ├── 20260415_1300/
│
├── shared/
│   ├── .env
│   ├── storage/
│   │   ├── logs/
│   │   ├── framework/
│   │   └── uploads/
│
├── current -> releases/20260415_1300

---

## 🧠 Core Idea

- Each deploy = new folder inside `releases/`
- `current` always points to the active version
- Shared data (env, storage) stays persistent
- Switching versions = changing symlink only

---

## ⚙️ Deployment Flow

### 1. Create new release
releases/20260415_1400/

---

### 2. Clone or upload code
git clone <repo> releases/20260415_1400

---

### 3. Install dependencies
composer install
npm install

---

### 4. Link shared files
ln -s shared/.env releases/20260415_1400/.env
ln -s shared/storage releases/20260415_1400/storage

---

### 5. Run migrations
php artisan migrate --force

---

### 6. Activate new version (zero downtime)
ln -sfn releases/20260415_1400 current

---

## 🔁 Rollback (Instant)

ln -sfn releases/20260415_1200 current

---

## ✅ Advantages

- Zero downtime deployments
- Instant rollback
- Clean architecture
- CI/CD friendly
- Production-grade structure

---

## ❌ Disadvantages

- Requires SSH access
- Slightly complex at first
- Requires symlink understanding

---

## 🏁 Summary

- releases = versions
- shared = persistent data
- current = active version pointer
