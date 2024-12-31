# HA MariaDB Galera Cluster và MaxScale

## 1. MariaDB Galera Cluster

**MariaDB Galera Cluster** là một giải pháp cơ sở dữ liệu phân tán cung cấp tính năng **Multi-Master Replication** (sao chép đa chủ) đồng bộ. Mỗi node (nút) trong cụm đều có quyền đọc/ghi dữ liệu và đảm bảo dữ liệu nhất quán trên tất cả các node.

### **Đặc điểm chính của Galera Cluster**
1. **Replication đồng bộ**:
   - Dữ liệu được đồng bộ hóa ngay lập tức trên tất cả các node.
   - Giảm rủi ro mất mát dữ liệu khi một node bị lỗi.

2. **Multi-Master Replication**:
   - Tất cả các node đều có thể xử lý các yêu cầu đọc/ghi.
   - Hỗ trợ cân bằng tải giữa các node.

3. **Scalability**:
   - Hệ thống dễ dàng mở rộng bằng cách thêm node vào cluster.

4. **Tính khả dụng cao (HA)**:
   - Nếu một node bị lỗi, cluster vẫn hoạt động bình thường.

5. **Chức năng Quorum**:
   - Cluster sử dụng cơ chế kiểm tra số lượng node hoạt động (quorum) để xác định trạng thái của cluster.
   - Một node không thể hoạt động đơn lẻ nếu không đạt được quorum.

### **Kiến trúc**
- Tối thiểu cần 3 node để đạt được tính HA và tránh split-brain (xung đột giữa các node).
- Sử dụng **Galera Arbitrator** (GARBD) nếu không muốn sử dụng 3 node vật lý.

### **Ưu điểm**
- Tính sẵn sàng cao và đảm bảo tính toàn vẹn dữ liệu.
- Phân tải hiệu quả trong môi trường có lưu lượng lớn.

### **Nhược điểm**
- Có độ trễ khi sao chép dữ liệu do tính năng đồng bộ.
- Không phù hợp cho các ứng dụng yêu cầu khối lượng ghi dữ liệu cực lớn.

---

## 2. MariaDB MaxScale

**MaxScale** là một proxy middleware được phát triển bởi MariaDB. Nó cung cấp khả năng cân bằng tải, bảo mật, và quản lý kết nối cho một cụm cơ sở dữ liệu MariaDB Galera Cluster.

### **Chức năng chính của MaxScale**
1. **Load Balancing**:
   - Phân phối các yêu cầu giữa các node trong cluster để tối ưu hóa hiệu suất.

2. **Query Routing**:
   - Định tuyến truy vấn đọc (READ) đến các node phụ.
   - Gửi truy vấn ghi (WRITE) đến node chính.

3. **Failover Handling**:
   - Tự động chuyển hướng các kết nối nếu một node bị lỗi.

4. **Database Firewall**:
   - Lọc và bảo vệ chống lại các truy vấn độc hại.

5. **Session Management**:
   - Duy trì trạng thái phiên (session) ngay cả khi xảy ra lỗi ở node.

### **Ưu điểm**
- Tăng tính sẵn sàng và hiệu suất của cụm cơ sở dữ liệu.
- Hỗ trợ kiến trúc **Read-Write Splitting**.
- Quản lý dễ dàng với giao diện quản trị.

### **Nhược điểm**
- Cần cấu hình chính xác để tránh lỗi routing.
- Tăng độ phức tạp của hệ thống.

---

## So sánh MariaDB Galera Cluster và MaxScale

| Đặc điểm         | MariaDB Galera Cluster       | MariaDB MaxScale            |
|------------------|------------------------------|-----------------------------|
| **Chức năng**    | Sao chép và đồng bộ dữ liệu | Cân bằng tải, định tuyến    |
| **HA**           | Đảm bảo tính HA trực tiếp   | Cung cấp HA thông qua routing |
| **Quy mô**       | Phù hợp với cụm nhiều node  | Tăng hiệu suất trong môi trường lớn |
| **Ứng dụng**     | Cụm cơ sở dữ liệu phân tán  | Proxy cho cơ sở dữ liệu     |

---

## Ứng dụng thực tế

1. **Galera Cluster**:
   - Triển khai trong môi trường yêu cầu tính toàn vẹn dữ liệu cao.
   - Các hệ thống ERP, CRM, hoặc các ứng dụng có khối lượng giao dịch lớn.

2. **MaxScale**:
   - Sử dụng như một lớp trung gian để cải thiện hiệu suất và quản lý kết nối.
   - Tăng hiệu quả cho các cụm Galera Cluster hoặc MariaDB độc lập.

---
