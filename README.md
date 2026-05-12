# BT3-Developing-Applications-With-Open-Source
# SỬ DỤNG WORDPRESS ĐỂ TẠO WEBSITE
- Môn học: Phát triển ứng dụng với mã nguồn mở - TEE0421
- Họ và tên: Ngụy Đình Tuấn Hà
- Công nghệ sử dụng: Wordpress, Docker, MariaDB, PhpMyAdmin, Cloudflare
# 📌 MỤC LỤC
## 1. Tạo thư mục dự án
Trước hết, hãy tạo một thư mục riêng để quản lý toàn bộ file cấu hình:
```
mkdir wordpress-project
cd wordpress-project
mkdir db-data wp-data
```
<img width="923" height="122" alt="image" src="https://github.com/user-attachments/assets/fa0ef2e2-c80d-4ca0-acd2-b703b4c11950" />

## 2. Tạo file docker-compose.yml
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

## 3. Public Web bằng Cloudflare Tunnel
### Bước 1: Truy cập trang Cloudflare Zero Trust tạo Tunnel mới

<img width="1919" height="1037" alt="image" src="https://github.com/user-attachments/assets/ce0c7840-2fce-4ca1-b783-99721b83c8ea" />

### Bước 2: Copy đoạn mã sau chữ --token  trong ô Run Tunnel with Docker
### Bước 3: Mở file docker-compose để dán mã vào
Sử dụng cloudflare tunnel dưới dạng docker container thay vì cài đặt cloudflare trên ubuntu để tiện quản lý
- Truy cập file docker-compose.yml và thêm đoạn mã sau:
```
tunnel:
    image: cloudflare/cloudflared:latest
    container_name: wp_tunnel
    restart: always
    # Thay <YOUR_TOKEN> bằng token lấy từ Cloudflare Zero Trust
    command: tunnel run --token <YOUR_TOKEN>
```
- Sau khi dán mã, chạy lại docker compose up -d để cập nhật
  
<img width="1117" height="627" alt="image" src="https://github.com/user-attachments/assets/62f01f0d-0699-4ea2-bff9-ede5c70dce7a" />

### Bước 4: Kiểm tra và thêm route published application
#### Sau khi chạy lại hệ thống thành công, connect status trên web báo successfull là ok -> bấm continue

<img width="1917" height="1040" alt="image" src="https://github.com/user-attachments/assets/6ad7fb9f-f117-4926-af47-b215af2ec5ea" />

#### thêm route published application
| Trường | Nội dung |
| :--- | :--- |
| subdomain | wordpress |
| domain | appwebtuanha.io.vn |
| service url | [http://wordpress:80](http://wordpress:80) |

<img width="1918" height="1040" alt="image" src="https://github.com/user-attachments/assets/e9c17a70-6a1e-4113-847a-ba85cc6ba161" />

<img width="1916" height="1036" alt="image" src="https://github.com/user-attachments/assets/ba324e16-9007-42a1-8bf9-df9312788271" />

### Kết quả public
Như vậy, sub-domain đã truy cập thành công vào wordpress:

<img width="1918" height="1038" alt="image" src="https://github.com/user-attachments/assets/b48779d8-a0e7-4850-97c0-f9e370e29d14" />

## 4. Tạo bài viết
### 4.1. Tạo 1 bài viết trong wordpress giới thiệu về bản thân sinh viên: thông tin cá nhân, sở thích, ... bài viết có thể chứa hình ảnh, âm thanh, video, ...
#### Truy cập url và thực hiện tạo tài khoản wordpress. Sau khi tạo xong thì login. Kết quả login:

<img width="1918" height="1040" alt="image" src="https://github.com/user-attachments/assets/cfd143fc-e30c-4a80-b426-9a552cb19c91" />

#### Vào mục Bài viết -> Tạo bài viết.

<img width="1919" height="1041" alt="image" src="https://github.com/user-attachments/assets/fde0acf9-83bf-432d-900a-32e11f23ebb9" />
