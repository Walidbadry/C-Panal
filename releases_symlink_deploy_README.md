# 🚀 Deployment Strategy: Releases + Symlink

## 📁 Project Structure

```
/home/username/app/

├── dev/                              # 🟢 Development Environment
│   ├── releases/
│   │   └── local_build/
│   ├── shared/
│   │   ├── .env
│   │   └── storage/
│   ├── current -> releases/local_build
│   └── scripts/
│       └── run.sh
│
├── staging/                          # 🟡 Staging Environment
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
├── prod/                             # 🔵 Production Environment
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
│   │       ├── sessions/
│   │       └── uploads/
│   │
│   ├── current -> releases/20260415_1400
│   └── scripts/
│       ├── deploy.sh
│       ├── rollback.sh
│       └── healthcheck.sh
│
│
├── public_html/                      # 🌐 Web Entry Point (Production)
│   ├── index.php
│   ├── .htaccess
│
├── staging.example.com/              # 🌐 Staging Web Root
│   ├── index.php
│   ├── .htaccess
│
├── shared_global/                    # 🧠 Optional Global Shared (Advanced)
│   ├── backups/
│   ├── logs/
│   └── ci-cache/
│
└── README.md

```

---
```
User Browser
   ↓
public_html/index.php
   ↓
current symlink
   ↓
releases/20260415_1600
   ↓
Laravel app boots
   ↓
Controller runs
   ↓
Response يرجع
```
## ⚙️ DEPLOY.SH WORKFLOW
deploymint.sh
```php
1. Create new release folder
2. Clone project
3. Install dependencies
4. Link shared files
5. Run migrations
6. Optimize Laravel
7. Switch current (symlink)
```
```
#!/bin/bash

DATE=$(date +"%Y%m%d_%H%M%S")

APP_DIR="/home/username/app/prod"
RELEASE_DIR="$APP_DIR/releases/$DATE"

echo "🚀 Creating new release: $DATE"

mkdir -p $RELEASE_DIR
cd $RELEASE_DIR

echo "📥 Cloning project..."
git clone https://github.com/your-repo.git .

echo "🔗 Linking shared files..."
ln -s $APP_DIR/shared/.env .env
ln -s $APP_DIR/shared/storage storage

echo "📦 Installing dependencies..."
composer install --no-dev --optimize-autoloader

echo "⚙️ Running migrations..."
php artisan migrate --force

echo "⚡ Optimizing Laravel..."
php artisan config:cache
php artisan route:cache
php artisan view:cache

echo "🔄 Switching release..."
ln -sfn $RELEASE_DIR $APP_DIR/current

echo "✅ Deployment finished successfully!"
```
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
