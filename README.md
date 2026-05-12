# BT3-Developing-Applications-With-Open-Source
# SỬ DỤNG WORDPRESS ĐỂ TẠO WEBSITE
- Môn học: Phát triển ứng dụng với mã nguồn mở - TEE0421
- Họ và tên: Ngụy Đình Tuấn Hà
- Công nghệ sử dụng: Wordpress, Docker, MariaDB, PhpMyAdmin, Cloudflare
# 📌 MỤC LỤC
1. Tạo thư mục dự án
Trước hết, hãy tạo một thư mục riêng để quản lý toàn bộ file cấu hình:
```
mkdir wordpress-project
cd wordpress-project
mkdir db-data wp-data
```
<img width="923" height="122" alt="image" src="https://github.com/user-attachments/assets/fa0ef2e2-c80d-4ca0-acd2-b703b4c11950" />

2. Tạo file docker-compose.yml
Để tránh trùng port với dự án Django cũ, em sẽ đổi port của phpMyAdmin thành 8081.
- Tạo file nano docker-compose.yml
- Thêm nội dung sau:
```
version: '3.8'

services:
  db:
    image: mariadb:latest
    container_name: wp_mariadb
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: '123456'
      MYSQL_DATABASE: wordpress_db
      MYSQL_USER: admin
      MYSQL_PASSWORD: '123456'
    volumes:
      - ./db-data:/var/lib/mysql

  phpmyadmin:
    image: phpmyadmin:latest
    container_name: wp_phpmyadmin
    restart: always
    ports:
      - "8081:80"
    environment:
      PMA_HOST: db
    depends_on:
      - db

  wordpress:
    image: wordpress:latest
    container_name: wp_site
    restart: always
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: admin
      WORDPRESS_DB_PASSWORD: '123456'
      WORDPRESS_DB_NAME: wordpress_db
    volumes:
      - ./wp-data:/var/www/html
    depends_on:
      - db
```
- Lưu lại Ctrl O -> Enter-> Ctrl X

<img width="1112" height="623" alt="image" src="https://github.com/user-attachments/assets/48a1fc67-4840-4cec-a0a2-ccffb7b586b1" />

- Chạy lệnh docker compose up -dđể ubuntu pull và khởi động service.
- Chạy docker ps để kiểm tra

<img width="1117" height="621" alt="image" src="https://github.com/user-attachments/assets/bb44359c-9ec0-40cd-8e44-bcbfef5ee620" />

## Kiểm tra các dịch vụ:
### Dịch vụ PhpMyAdmin 192.168.183.129:8081

<img width="1916" height="1040" alt="image" src="https://github.com/user-attachments/assets/609fea7c-b052-4e6f-abeb-6ea7408a8050" />

### Dịch vụ Wordpress 192.168.183.129:8080

<img width="1916" height="1041" alt="image" src="https://github.com/user-attachments/assets/713c5498-6c0e-40a8-9fc3-cbf62eac861b" />

