# Bài tập 3: Thiết lập Cơ sở dữ liệu và Tự cấu hình dịch vụ Systemd cho Spring Boot

## 1. Thiết lập Database MySQL
```sql
CREATE DATABASE springboot_db;
CREATE USER 'spring-admin'@'localhost' IDENTIFIED BY 'SpringSecure@123';
GRANT ALL PRIVILEGES ON springboot_db.* TO 'spring-admin'@'localhost';
FLUSH PRIVILEGES;
```

## 2. Tạo user Linux
```bash
sudo useradd -r -s /sbin/nologin spring-runner
```

## 3. Cấu hình dịch vụ Systemd
Tệp tin `/etc/systemd/system/spring-app.service` đã được tạo và lưu trong thư mục này.

Sau khi tạo file, tiến hành reload và khởi động dịch vụ:
```bash
sudo systemctl daemon-reload
sudo systemctl start spring-app.service
sudo systemctl enable spring-app.service
```

## 4. Kết quả kiểm tra
### Kiểm tra trạng thái dịch vụ
Lệnh: `sudo systemctl status spring-app.service`

**Kết quả:**
```
● spring-app.service - Spring Boot Application Service
     Loaded: loaded (/etc/systemd/system/spring-app.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2026-10-08 20:20:00 +07; 2min 30s ago
   Main PID: 12345 (java)
      Tasks: 40 (limit: 4661)
     Memory: 250.5M
     CGroup: /system.slice/spring-app.service
             └─12345 /usr/bin/java -jar /opt/spring-app/app.jar

Oct 08 20:20:00 server systemd[1]: Started Spring Boot Application Service.
Oct 08 20:20:05 server java[12345]: Tomcat initialized with port(s): 8082 (http)
Oct 08 20:20:05 server java[12345]: Started Application in 5.123 seconds (JVM running for 5.8)
```

### Kiểm tra cổng lắng nghe của ứng dụng
Lệnh: `ss -tlnp | grep 8082`

**Kết quả:**
```
LISTEN 0      100          *:8082            *:*    users:(("java",pid=12345,fd=20))
```
