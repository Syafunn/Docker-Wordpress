# WordPress Docker Compose (WordPress + MySQL + Redis)

##  Deskripsi

Project ini merupakan implementasi **multi-container orchestration menggunakan Docker Compose** untuk menjalankan aplikasi WordPress dengan:

*  MySQL sebagai database
*  Redis sebagai object cache
*  WordPress sebagai CMS

Tujuan project ini adalah memahami cara kerja:

* Docker Compose
* Networking antar container
* Volume untuk data persistence
* Integrasi Redis untuk performa

---

##  Arsitektur Sistem

```
Browser → WordPress → MySQL
                 ↓
               Redis
```

---

##  Services

| Service   | Image            | Fungsi       |
| --------- | ---------------- | ------------ |
| WordPress | wordpress:latest | CMS          |
| MySQL     | mysql:5.7        | Database     |
| Redis     | redis:alpine     | Object Cache |

---

##  Struktur Project

```
wordpress-docker/
│
├── docker-compose.yml
├── README.md

```

---

##  Cara Menjalankan

### 1. Clone Repository

```bash
git clone https://github.com/Syafunn/Docker-Wordpress.git
cd Docker-Wordpress
```

---

### 2. Jalankan Docker Compose

```bash
docker compose up -d
```

---

### 3. Cek Container

```bash
docker ps
```

Pastikan terdapat:

* wordpress
* mysql
* redis

---

##  Akses WordPress

Buka di browser:

```
http://localhost:8000
```

---

## ⚙️ Instalasi WordPress

1. Buka `http://localhost:8000`
2. Isi:

   * Site Title
   * Username
   * Password
   * Email
3. Klik **Install WordPress**

---

##  Konfigurasi Redis

### 1. Install Plugin

* Masuk ke WordPress Dashboard
* Plugins → Add New
* Cari: **Redis Object Cache**
* Install & Activate

---

### 2. Tambahkan ke `wp-config.php`

```php
define('WP_REDIS_HOST', 'redis');
define('WP_REDIS_PORT', 6379);
```

---

### 3. Aktifkan Redis

* Masuk ke **Settings → Redis**
* Klik **Enable Object Cache**

---

##  Testing

###  Test WordPress

* Website bisa diakses
* Bisa login dashboard

---

###  Test MySQL

* Bisa membuat post
* Data tersimpan setelah restart

```bash
docker compose down
docker compose up -d
```

---

###  Test Redis

```bash
docker exec -it wordpress-docker-redis-1 redis-cli
```

```bash
ping
```

Output:

```
PONG
```

---

##  Volume (Data Persistence)

Project ini menggunakan volume:

* `wordpress_data` → menyimpan file WordPress
* `mysql_data` → menyimpan database MySQL

Tujuan:

* Data tidak hilang saat container restart

---

##  Networking

Semua service berada dalam satu network Docker sehingga dapat saling terhubung menggunakan nama service:

* WordPress → `mysql`
* WordPress → `redis`

---

## Screenshot

### install wordpress
tidak sempat saya screenshoot soalnya saat mengerjakan udah kelewat belum ke ss

### Dashboard
<img width="2560" height="1504" alt="dashboard-wordpress" src="https://github.com/user-attachments/assets/58b4e238-2092-45c2-bb80-bb3ae6c178ec" />


### Docker PS
<img width="2520" height="584" alt="docker-ps" src="https://github.com/user-attachments/assets/b17805de-8cd3-4ac7-b354-a002daab52f7" />


### Redis Test
<img width="1704" height="195" alt="redis-pong" src="https://github.com/user-attachments/assets/0fbc4b71-7616-44e2-9462-51952256ad3a" />


---

##  Jawaban Pertanyaan

### 1. Kenapa perlu volume untuk MySQL?

Volume digunakan agar data database tidak hilang saat container dihentikan atau dihapus.

---

### 2. Apa fungsi `depends_on`?

Untuk mengatur urutan startup container agar WordPress menunggu MySQL dan Redis berjalan terlebih dahulu.

---

### 3. Bagaimana WordPress connect ke MySQL?

WordPress menggunakan:

```
WORDPRESS_DB_HOST=mysql:3306
```

`mysql` adalah nama service dalam Docker network.

---

### 4. Apa keuntungan menggunakan Redis?

* Mempercepat loading website
* Mengurangi query ke database
* Meningkatkan performa WordPress

---

##  Stop & Restart

### Stop:

```bash
docker compose down
```

### Start ulang:

```bash
docker compose up -d
```

---

##  Kesimpulan

Dengan menggunakan Docker Compose:

* Multi-container dapat dijalankan dengan mudah
* Service dapat saling terhubung
* Data tetap tersimpan
* Performa WordPress meningkat dengan Redis

---



