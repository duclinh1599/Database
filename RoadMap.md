# Lộ Trình Học Đọc Hiểu Code NestJS

## Tuần 1-2: Học JavaScript Cơ Bản
### Mục tiêu:
- Nắm vững cú pháp và các khái niệm cơ bản của JavaScript.

### Kiến thức cần học:
1. **Cú pháp cơ bản:**
   - Biến: `var`, `let`, `const`.
   - Kiểu dữ liệu: `string`, `number`, `boolean`, `array`, `object`.
   - Toán tử: `+`, `-`, `*`, `/`, `===`, `!==`, `&&`, `||`.
   - Điều kiện: `if`, `else`, `switch`.
   - Vòng lặp: `for`, `while`, `forEach`.

2. **Nâng cao:**
   - Hàm: `function`, arrow function.
   - Scope, closure, hoisting.
   - Prototype và kế thừa.
   - Lập trình bất đồng bộ: Callback, Promises, async/await.

3. **Thực hành:**
   - Tạo một ứng dụng nhỏ như To-Do list hoặc gọi API hiển thị dữ liệu.

---

## Tuần 3: Học Node.js Cơ Bản
### Mục tiêu:
- Hiểu cách Node.js hoạt động và làm quen với các module quan trọng.

### Kiến thức cần học:
1. **Tổng quan Node.js:**
   - Node.js là gì? Điểm khác biệt với trình duyệt JavaScript.
   - Hiểu về **event loop** và **non-blocking I/O**.

2. **Các module quan trọng:**
   - `http`: Tạo một server đơn giản.
   - `fs`: Đọc/ghi file.
   - `path`: Làm việc với đường dẫn.
   - `events`: Hiểu cách hoạt động của EventEmitter.

3. **Quản lý module:**
   - Sử dụng `npm` để cài đặt và quản lý thư viện.
   - Tạo và sử dụng module tự viết.

4. **Lập trình bất đồng bộ:**
   - Callback, Promises, async/await.

### Thực hành:
- Tạo một API REST đơn giản:
  - Một endpoint trả về dữ liệu JSON (dùng `http` module).
  - Đọc/ghi dữ liệu từ file JSON bằng `fs`.

---

## Tuần 4: Học TypeScript Cơ Bản
### Mục tiêu:
- Hiểu cú pháp và cách làm việc với TypeScript, chuẩn bị cho NestJS.

### Kiến thức cần học:
1. **Cú pháp TypeScript:**
   - Kiểu dữ liệu: `string`, `number`, `boolean`, `array`, `tuple`, `enum`, `any`.
   - Interface, class, và inheritance.
   - Generics.
   - Decorators (quan trọng trong NestJS).

2. **Thực hành:**
   - Viết các bài tập nhỏ: Tạo class, interface, và function với kiểu dữ liệu cụ thể.
   - Chuyển đổi một dự án nhỏ từ JavaScript sang TypeScript.

---

## Tuần 5-7: Làm Quen Với NestJS
### Mục tiêu:
- Hiểu kiến trúc của NestJS và cách tổ chức dự án.

### Kiến thức cần học:
1. **Kiến trúc NestJS:**
   - Module, Controller, Service (3 thành phần chính).
   - Dependency Injection.
   - Middleware, Guards, Interceptors.
   - Pipes và Exception Filters.

2. **Thực hành:**
   - Tạo một ứng dụng REST API đơn giản (CRUD) với các thành phần trên.
   - Kết nối API với cơ sở dữ liệu MariaDB/MySQL bằng TypeORM.

---

## Tuần 8: Hệ Sinh Thái Liên Quan
### Mục tiêu:
- Làm quen với các công cụ và thư viện thường dùng với NestJS.

### Kiến thức cần học:
1. **Cơ sở dữ liệu:**
   - Hiểu cơ bản về MariaDB/MySQL.
   - Sử dụng TypeORM để kết nối và thực hiện các thao tác CRUD.

2. **Authentication:**
   - Sử dụng Passport.js để xây dựng hệ thống xác thực (JWT).

### Thực hành:
- Xây dựng một ứng dụng quản lý công việc (Task Management):
  - Tạo, đọc, sửa, xóa (CRUD).
  - Đăng nhập bằng JWT.

---

## Tuần 9 Trở Đi: Đọc Hiểu Code NestJS Thực Tế
### Mục tiêu:
- Làm quen với các dự án NestJS thực tế và phân tích mã nguồn.

### Học cách đọc và phân tích:
1. **Đọc dự án mã nguồn mở NestJS trên GitHub:**
   - Tìm hiểu cách tổ chức module, controller, và service.
   - Ghi chú và thử viết lại các chức năng chính.

2. **Thực hành tự giải thích:**
   - Tạo tài liệu hoặc sơ đồ giải thích các thành phần của dự án.

---

## Tài Liệu Tham Khảo
- Youtube JavaScript:
   - https://www.youtube.com/watch?v=Ko3jE3HNtFw&list=PLP6tw4Zpj-RLU3QE246A8zDliE7k_-dA5
   - https://www.youtube.com/watch?v=NoPezyF1jxc&list=PLodO7Gi1F7R0u7LAtcBnSLJupZwGJZn2C&index=5
- **JavaScript:**
  - [JavaScript.info](https://javascript.info/)
  - [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- **TypeScript:**
  - [TypeScript Documentation](https://www.typescriptlang.org/docs/)
- **NestJS:**
  - [NestJS Documentation](https://docs.nestjs.com/)
