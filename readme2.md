# 🚀 Laravel Multi-Environment Deployment on cPanel (Without SSH)

This project demonstrates a **professional deployment architecture** for Laravel applications on **cPanel without SSH access**, using a **Hybrid Versioning System (versions + txt switching)**.

---

## 📌 Overview

Instead of traditional deployments, this setup uses:

* 🔵 Versioned releases (`versions/`)
* 🟢 Shared resources (`shared/`)
* 🟡 Environment-based version switching (`envs/*.txt`)
* 🌍 Separate entry points per environment (`public_html`, `staging`, `dev`)

This allows:

* ✅ Zero downtime deployments
* ✅ Instant rollback
* ✅ Multi-environment support
* ✅ Works بالكامل على cPanel بدون SSH

---

## 📁 Project Structure

```
/home/username/
│
├── versions/                # All application versions
│   ├── v1/
│   ├── v2/
│   └── v3/
│
├── shared/                 # Shared resources
│   ├── .env.dev
│   ├── .env.staging
│   ├── .env.prod
│   ├── storage/
│
├── envs/                   # Active version per environment
│   ├── dev.txt
│   ├── staging.txt
│   └── prod.txt
│
├── public_html/            # Production entry
│   └── index.php
│
├── staging/                # Staging entry
│   └── index.php
│
└── dev/                    # Development entry
    └── index.php
```

---

## 🌍 Environments Setup

| Environment | Domain             | Path        |
| ----------- | ------------------ | ----------- |
| Production  | domain.com         | public_html |
| Staging     | staging.domain.com | /staging    |
| Development | dev.domain.com     | /dev        |

---

## ⚙️ How It Works

Each environment has its own `index.php` that:

1. Reads the active version from:

   ```
   envs/{env}.txt
   ```
2. Loads the corresponding version from:

   ```
   versions/{version}
   ```
3. Uses the correct environment file:

   ```
   shared/.env.{env}
   ```
4. Shares storage:

   ```
   shared/storage/
   ```

---

## 🔥 Example: index.php Logic

```php
$env = 'prod';

$version = trim(file_get_contents(__DIR__ . "/../envs/{$env}.txt"));
$appPath = __DIR__ . "/../versions/{$version}";

$envFile = __DIR__ . "/../shared/.env.{$env}";
$storagePath = __DIR__ . "/../shared/storage";


require $appPath . '/vendor/autoload.php';
$app = require_once $appPath . '/bootstrap/app.php';
$app->useEnvironmentPath(dirname($envFile));
$app->loadEnvironmentFrom(basename($envFile));
$app->useStoragePath($storagePath);

$kernel = $app->make(Illuminate\Contracts\Http\Kernel::class);

$response = $kernel->handle(
    $request = Illuminate\Http\Request::capture()
)->send();

$kernel->terminate($request, $response);
```

---

## 🚀 Deployment Flow

### 1. Upload New Version

```
versions/v4/
```

---

### 2. Test on Dev

```
envs/dev.txt = v4
```

👉 Open:

```
dev.domain.com
```

---

### 3. Move to Staging

```
envs/staging.txt = v4
```

---

### 4. Deploy to Production

```
envs/prod.txt = v4
```

🔥 Users are instantly switched to the new version (zero downtime)

---

## 🔁 Rollback

If an issue occurs:

```
envs/prod.txt = v3
```

✅ Immediate rollback

---

## ⚡ Advanced Options

### Separate Storage per Environment

```
shared/
  storage_dev/
  storage_staging/
  storage_prod/
```

Modify:

```php
$storagePath = "/shared/storage_{$env}";
```

---

## ⚖️ Advantages

* ✔ No SSH required
* ✔ Zero downtime deployment
* ✔ Instant rollback
* ✔ Clean environment separation
* ✔ قريب جدًا من أنظمة الشركات

---

## ❌ Limitations

* ❌ Manual switching
* ❌ Not as clean as symlink-based deployments
* ❌ Requires careful file management

---

## 🧠 Real-World Use Case

This setup simulates modern deployment strategies used in:

* Versioned releases
* Blue/Green deployment concepts
* CI/CD pipelines (manually adapted for cPanel)

---

## 🎯 Conclusion

This architecture is ideal for:

* Shared hosting (cPanel)
* Laravel projects without SSH
* Developers who want **professional deployment practices** in limited environments

---

## 👨‍💻 Author

Built for learning and real-world DevOps simulation 🚀
