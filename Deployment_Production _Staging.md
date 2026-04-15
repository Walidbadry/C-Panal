# 🚀 Laravel Deployment (Production / Staging) on cPanel

## 📌 Overview

This project demonstrates a **traditional deployment strategy** for a Laravel application using **Production** and **Staging** environments on **cPanel (without SSH access)**.

---

## 🧠 Concept

We maintain **two separate environments**:

* 🔵 **Production** → Live application (used by real users)
* 🟠 **Staging** → Testing environment (used for development & QA)

### 🔄 Workflow

1. Develop and test features in **Staging**
2. Verify everything works correctly
3. Manually deploy changes to **Production**

---

## 📁 Project Structure

```
/home/username/
│
├── laravel_prod/                # 🔵 Production code
│   ├── app/
│   ├── bootstrap/
│   ├── config/
│   ├── public/
│   ├── resources/
│   ├── routes/
│   ├── vendor/
│   └── .env
│
├── laravel_staging/             # 🟠 Staging code
│   ├── app/
│   ├── bootstrap/
│   ├── config/
│   ├── public/
│   ├── resources/
│   ├── routes/
│   ├── vendor/
│   └── .env
│
├── public_html/                 # 🌍 Main domain root
│   └── index.php
│
└── staging.example.com/         # 🧪 Subdomain root
    └── index.php
```

---

## ⚙️ Environment Configuration

### 🔵 Production

File: `public_html/index.php`

```php
require __DIR__.'/../laravel_prod/vendor/autoload.php';
$app = require_once __DIR__.'/../laravel_prod/bootstrap/app.php';
```

### 🟠 Staging

File: `staging.example.com/index.php`

```php
require __DIR__.'/../laravel_staging/vendor/autoload.php';
$app = require_once __DIR__.'/../laravel_staging/bootstrap/app.php';
```

---

## 🗄️ Database Setup

Use separate databases for each environment:

| Environment | Database   |
| ----------- | ---------- |
| Production  | db_prod    |
| Staging     | db_staging |

### Example `.env` files

#### 🟠 Staging

```
APP_ENV=staging
APP_DEBUG=true
DB_DATABASE=db_staging
```

#### 🔵 Production

```
APP_ENV=production
APP_DEBUG=false
DB_DATABASE=db_prod
```

---

## 🚀 Deployment Steps

### Step 1: Develop on Staging

* Implement new features
* Test API endpoints
* Verify UI/UX

### Step 2: Deploy to Production

* Open **cPanel File Manager**
* Copy files from:

  ```
  laravel_staging/
  ```

  to:

  ```
  laravel_prod/
  ```

---

## ⚠️ Limitations

* ❌ Possible downtime during deployment
* ❌ No automatic rollback
* ❌ Manual process (error-prone)
* ❌ Not scalable for large systems

---

## ✅ When to Use

* Small projects
* Learning purposes
* Shared hosting (cPanel)
* No SSH access

---

## 💡 Best Practices

* 🔒 Do NOT overwrite `.env` files
* 💾 Always take backups before deployment
* 🗄️ Use separate databases
* ⚠️ Test thoroughly in staging before deploying

---

## 🔮 Future Improvements

* 🔵 Zero-downtime deployment (Releases + Symlink)
* 🟠 CI/CD pipeline integration
* 🟢 Docker-based deployment
* ☁️ Migration to VPS or Cloud (AWS, DigitalOcean)

---

## 👨‍💻 Author

Walid Badry
Junior DevOps Engineer 🚀

---
