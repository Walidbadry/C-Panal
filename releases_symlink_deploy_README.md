# 🚀 Deployment Strategy: Releases + Symlink

## 📁 Project Structure

```
/home/username/app/
│
├── prod/                              # 🔵 Production Environment
│   ├── releases/
│   │   ├── 20260415_1200/
│   │   ├── 20260415_1300/
│   │   └── 20260415_1400/
│   │
│   ├── shared/
│   │   ├── .env
│   │   └── storage/
│   │       ├── logs/
│   │       ├── cache/
│   │       └── sessions/
│   │
│   ├── current -> releases/20260415_1400
│   └── scripts/
│       ├── deploy.sh
│       └── rollback.sh
│
│
├── staging/                           # 🟡 Staging Environment
│   ├── releases/
│   │   ├── 20260415_1200/
│   │   └── 20260415_1300/
│   │
│   ├── shared/
│   │   ├── .env
│   │   └── storage/
│   │
│   ├── current -> releases/20260415_1300
│   └── scripts/
│       ├── deploy.sh
│       └── rollback.sh
│
│
├── dev/                               # 🟢 Development Environment (optional)
│   ├── releases/
│   │   └── local_build/
│   │
│   ├── shared/
│   │   ├── .env
│   │   └── storage/
│   │
│   ├── current -> releases/local_build
│   └── scripts/
│       └── run.sh
│
│
└── README.md

```

---

## 💡 Concept

* Each deployment creates a **new release folder**
* The application runs from a symlink called:

```
current/
```

* This symlink always points to the **latest stable release**

---

## ⚙️ Deployment Flow

### 1. Create New Release

```
cd app/releases
mkdir 20260415_1400
cd 20260415_1400
```

---

### 2. Pull Code

```
git clone <repo_url> .
# or
git pull origin main
```

---

### 3. Install Dependencies

#### PHP (Laravel)

```
composer install --no-dev --optimize-autoloader
```

#### Node.js

```
npm install
npm run build
```

---

### 4. Link Shared Resources

```
ln -s ../../shared/.env .env
rm -rf storage
ln -s ../../shared/storage storage
```

---

### 5. Run Migrations

```
php artisan migrate --force
```

---

### 6. Switch to New Release (Zero Downtime)

```
ln -sfn releases/20260415_1400 current
```

---

## 🔄 Rollback

To revert to a previous release:

```
ln -sfn releases/20260415_1300 current
```

---

## ✅ Advantages

* 🚀 Zero downtime deployment
* 🔄 Instant rollback
* 🧼 Clean architecture
* 🤖 CI/CD friendly
* 💼 Production standard

---

## ❌ Disadvantages

* Requires SSH access
* Slightly more complex setup
* Consumes more disk space

---

## 🧠 Best Practices

### Keep only last 5 releases

```
ls -dt releases/* | tail -n +6 | xargs rm -rf
```

---

### Use clear naming

```
YYYY-MM-DD_HH-MM
```

---

### Cache optimization (Laravel)

```
php artisan config:cache
php artisan route:cache
php artisan view:cache
```

---

## 🔥 Summary

This deployment strategy ensures:

* Safe deployments
* Instant switching
* Easy rollback

It is widely used in production environments for reliability and scalability.
