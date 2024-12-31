# Kiểm Tra Kiến Thức Docker

## **Phần 1: Câu hỏi cơ bản**

### 1. Docker là gì? Nó khác gì so với máy ảo (Virtual Machine)?
- **Docker:** Một nền tảng giúp đóng gói, phân phối, và chạy các ứng dụng dưới dạng container. Docker sử dụng công nghệ containerization để cô lập ứng dụng, chia sẻ kernel với hệ điều hành máy chủ (host).
- **Khác biệt với máy ảo:**
  - Docker nhẹ hơn vì không cần cài đặt một hệ điều hành đầy đủ bên trong container.
  - Docker khởi động nhanh hơn và sử dụng tài nguyên hiệu quả hơn so với máy ảo.

### 2. Docker Image và Docker Container khác nhau ở điểm nào?
- **Docker Image:** Là một bản mẫu (template) bất biến, chứa mọi thứ cần thiết để chạy ứng dụng, bao gồm mã nguồn, thư viện, và môi trường.
- **Docker Container:** Là một phiên bản chạy của Docker Image. Container là một thực thể động, có thể đọc/ghi dữ liệu.

### 3. Làm thế nào để kiểm tra danh sách các container đang chạy trên hệ thống?
- Dùng lệnh:
  ```bash
  docker ps
  ```
- Nếu muốn xem cả container đang chạy và container đã dừng:
  ```bash
  docker ps -a
  ```

### 4. Câu lệnh nào dùng để xây dựng (build) một Docker image từ Dockerfile?
- Lệnh:
  ```bash
  docker build -t <image_name>:<tag> .
  ```
- Trong đó:
  - `<image_name>`: Tên của image.
  - `<tag>`: Phiên bản (ví dụ: `latest`).

### 5. Làm thế nào để gắn kết (bind) thư mục từ máy host vào container?
- Sử dụng cờ `-v` hoặc `--mount` khi chạy container:
  ```bash
  docker run -v /path/on/host:/path/in/container <image_name>
  ```
- Ví dụ:
  ```bash
  docker run -v /data:/app/data my-app
  ```

---

## **Phần 2: Câu hỏi về thao tác và quản lý**

### 6. Nếu một container bị dừng, làm thế nào để khởi động lại nó?
- Dùng lệnh:
  ```bash
  docker start <container_id_or_name>
  ```

### 7. Câu lệnh nào được sử dụng để xóa tất cả các container đã dừng?
- Lệnh:
  ```bash
  docker container prune
  ```

### 8. Làm thế nào để xem log của một container đang chạy?
- Dùng lệnh:
  ```bash
  docker logs <container_id_or_name>
  ```

### 9. Làm thế nào để chạy một container và cho phép nó tự động khởi động lại khi host được reboot?
- Dùng cờ `--restart`:
  ```bash
  docker run --restart=always <image_name>
  ```

### 10. Bạn có thể sử dụng lệnh gì để xóa tất cả Docker images không sử dụng?
- Lệnh:
  ```bash
  docker image prune
  ```
- Để xóa tất cả images không cần thiết:
  ```bash
  docker image prune -a
  ```

---

## **Phần 3: Câu hỏi về mạng**

### 11. Docker có các loại mạng nào? Hãy giải thích ngắn gọn về từng loại.
- **Bridge (mặc định):** Container có thể giao tiếp với nhau thông qua bridge network.
- **Host:** Container sử dụng trực tiếp mạng của máy chủ.
- **None:** Container không có mạng.
- **Overlay:** Dùng để kết nối container trên các host khác nhau (trong swarm).
- **Macvlan:** Container có địa chỉ MAC riêng.

### 12. Làm thế nào để chỉ định một container sử dụng một mạng Docker cụ thể khi chạy nó?
- Dùng cờ `--network`:
  ```bash
  docker run --network <network_name> <image_name>
  ```

### 13. Bạn có thể mô tả cách hai container trong cùng một mạng Docker giao tiếp với nhau?
- Nếu hai container nằm trong cùng một mạng (ví dụ: `bridge`), chúng có thể giao tiếp với nhau bằng cách sử dụng tên container như hostname.

---

## **Phần 4: Câu hỏi về Docker Compose**

### 14. Docker Compose là gì? Khi nào nên sử dụng nó?
- **Docker Compose:** Một công cụ dùng để định nghĩa và quản lý nhiều container cùng lúc, thông qua tệp `docker-compose.yml`.
- **Khi nên dùng:** Khi cần triển khai các ứng dụng có nhiều container (ví dụ: ứng dụng với backend, frontend, và database).

### 15. Trong tệp `docker-compose.yml`, bạn khai báo các dịch vụ (services) như thế nào?
- Ví dụ:
  ```yaml
  version: '3'
  services:
    app:
      image: my-app
      ports:
        - "8080:80"
    db:
      image: mysql
      environment:
        MYSQL_ROOT_PASSWORD: example
  ```

### 16. Câu lệnh nào dùng để khởi động tất cả các dịch vụ trong Docker Compose?
- Lệnh:
  ```bash
  docker-compose up
  ```

### 17. Làm thế nào để cập nhật một dịch vụ trong Docker Compose mà không làm gián đoạn toàn bộ hệ thống?
- Lệnh:
  ```bash
  docker-compose up <service_name> --no-deps
  ```

---

## **Phần 5: Câu hỏi nâng cao**

### 18. Làm thế nào để giảm kích thước của Docker image?
- **Sử dụng Alpine Linux:** Chọn base image nhỏ như `alpine`.
- **Multi-stage builds:** Chỉ copy các file cần thiết vào image cuối cùng.
- **Xóa cache:** Xóa các file tạm hoặc cache trong Dockerfile.

### 19. Docker Volume khác gì so với Bind Mount? Khi nào nên sử dụng Docker Volume?
- **Docker Volume:** Được quản lý bởi Docker. Dữ liệu lưu trong volume tồn tại dù container bị xóa.
- **Bind Mount:** Kết nối trực tiếp thư mục từ máy host vào container.
- **Khi nên dùng Docker Volume:** Khi cần lưu dữ liệu bền vững mà không phụ thuộc vào cấu trúc máy host.

### 20. Hãy giải thích khái niệm multi-stage builds trong Dockerfile.
- **Multi-stage builds:** Cho phép sử dụng nhiều giai đoạn trong Dockerfile để tối ưu hóa image cuối cùng. 
- Ví dụ:
  ```dockerfile
  FROM golang:1.16 AS builder
  WORKDIR /app
  COPY . .
  RUN go build -o main .

  FROM alpine:latest
  COPY --from=builder /app/main /app/main
  CMD ["/app/main"]
  
