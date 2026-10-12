

# BÀI 5: KHỞI CHẠY DỊCH VỤ ĐƠN GIẢN VỚI DOCKER COMPOSE

- **Môn học:** DevOps / Containerization
- **Bài thực hành:** Session 09 - Bài 5
- **Đường dẫn nộp bài:** `homework/session_09/ex5/`
- **Tệp nộp bài:** `docker-compose.yml`, `README.md`
- **Mục tiêu:** Viết tệp `docker-compose.yml` tối giản để định nghĩa và quản lý vòng đời của một dịch vụ web Nginx bằng Docker Compose CLI v2 (`up`, `ps`, `curl`, `down`).

---

## 1. Cấu trúc thư mục

```text
homework/session_09/ex5/
├── docker-compose.yml
└── README.md
```

---

## 2. Cấu hình tệp `docker-compose.yml`

```yaml
services:
  web:
    image: nginx:alpine
    ports:
      - "8082:80"
```

> **Giải thích cấu hình:**
> - `services`: Khai báo danh sách các dịch vụ sẽ khởi chạy.
> - `web`: Tên của dịch vụ (service name).
> - `image: nginx:alpine`: Sử dụng base image Nginx phiên bản Alpine Linux gọn nhẹ.
> - `ports: - "8082:80"`: Ánh xạ cổng `8082` của máy Host vào cổng `80` bên trong container. Cú pháp thụt lùi 2 khoảng trắng (space) chuẩn YAML.

---

## 3. Các bước thực hiện & Kết quả Terminal

### Bước 1: Khởi chạy dịch vụ ở chế độ nền (`docker compose up -d`)

Lệnh thực hiện:
```bash
docker compose up -d
```

**Kết quả đầu ra (Terminal Output):**
```text
[+] Running 2/2
 ✔ Network ex5_default  Created                                                    0.1s 
 ✔ Container ex5-web-1  Started                                                    0.2s 
```

> **Ghi chú:** Docker Compose tự động tạo một network mặc định (`ex5_default`) và khởi chạy container `ex5-web-1` ở chế độ detached (`-d`).

---

### Bước 2: Kiểm tra trạng thái dịch vụ (`docker compose ps`)

Lệnh thực hiện:
```bash
docker compose ps
```

**Kết quả đầu ra (Terminal Output):**
```text
NAME        IMAGE          COMMAND                  SERVICE   CREATED          STATUS          PORTS
ex5-web-1   nginx:alpine   "/docker-entrypoint.…"   web       10 seconds ago   Up 10 seconds   0.0.0.0:8082->80/tcp
```

> **Kết quả mong đợi:** Lệnh `docker compose ps` hiển thị rõ service `web` đang chạy (trạng thái `Up`) và cổng `8082` đã được forward tới cổng `80` trong container (`0.0.0.0:8082->80/tcp`).

---

### Bước 3: Gửi truy vấn HTTP kiểm tra trang web (`curl http://localhost:8082`)

Lệnh thực hiện:
```bash
curl http://localhost:8082
```

**Kết quả đầu ra (Terminal Output):**
```html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

> **Kết quả mong đợi:** Lệnh `curl` phản hồi mã HTML chứa trang chào mừng mặc định của Nginx với tiêu đề `<h1>Welcome to nginx!</h1>`.

---

### Bước 4: Hạ và dọn dẹp dịch vụ (`docker compose down`)

Lệnh thực hiện:
```bash
docker compose down
```

**Kết quả đầu ra (Terminal Output):**
```text
[+] Running 2/2
 ✔ Container ex5-web-1  Removed                                                    0.3s 
 ✔ Network ex5_default  Removed                                                    0.1s 
```

Kiểm tra lại trạng thái sau khi hạ dịch vụ:
```bash
docker compose ps
```

**Kết quả đầu ra:**
```text
NAME      IMAGE     COMMAND   SERVICE   CREATED   STATUS    PORTS
```
*(Tất cả container và network thuộc compose project đã được dọn dẹp sạch sẽ).*

---

## 4. Tổng kết đánh giá

| Tiêu chí | Lệnh thực hiện | Kết quả | Trạng thái |
| :--- | :--- | :--- | :---: |
| Tệp cấu hình | `docker-compose.yml` | Chuẩn định dạng YAML (2 spaces, không tab) | Đạt |
| Khởi chạy nền | `docker compose up -d` | Khởi tạo network và chạy ngầm container | Đạt |
| Kiểm tra trạng thái | `docker compose ps` | Hiển thị service `web` trạng thái `Up`, port `8082` | Đạt |
| Kiểm tra phản hồi | `curl http://localhost:8082` | Trả về trang "Welcome to nginx!" | Đạt |
| Dọn dẹp tài nguyên | `docker compose down` | Container và Network được gỡ bỏ hoàn toàn | Đạt |
