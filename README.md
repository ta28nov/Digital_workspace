# Digital Workspace Pro - Không Gian Làm Việc Số

## 1. Giới thiệu

**Digital Workspace Pro** là giải pháp không gian làm việc số toàn diện giúp doanh nghiệp hiện đại nâng cao năng suất và cộng tác. Ứng dụng cung cấp một Backend API mạnh mẽ và một ứng dụng di động đa nền tảng (iOS/Android) cho phép quản lý công việc, tài liệu, dự án, giao tiếp, và cập nhật thông tin hiệu quả. Giao diện trực quan, thân thiện hỗ trợ tối đa cho công việc cá nhân và theo nhóm.

---

## 2. Công nghệ sử dụng

### Backend (`BE/`)
- **Ngôn ngữ:** JavaScript (Node.js)
- **Framework:** Express.js
- **Database:** MongoDB (Mongoose ODM)
- **Auth:** JWT
- **File upload:** Multer (lưu cục bộ tại `uploads`)
- **Real-time:** Socket.IO (thông báo, chat)
- **Logging:** Winston
- **Khác:** bcryptjs, dotenv, express-async-handler, cors

### Frontend (`FE/`)
- **Ngôn ngữ:** TypeScript
- **Framework:** React Native (Expo)
- **Quản lý state:** React Context API, React Hooks
- **Điều hướng:** React Navigation
- **UI Components:** React Native Paper, @react-native-community/datetimepicker
- **Icons:** @expo/vector-icons
- **API Client:** Axios
- **Font:** expo-font
- **Bảo mật:** expo-secure-store
- **Thao tác ngày:** date-fns

---

## 3. Tính năng nổi bật

### 3.1. Xác thực người dùng
- Đăng ký, đăng nhập, lấy thông tin cá nhân (`/auth/me`)
- Quên mật khẩu (FE)
- Splash Screen, Onboarding

### 3.2. Quản lý người dùng
- Xem/cập nhật thông tin cá nhân
- Tìm kiếm, xem thông tin công khai người dùng khác

### 3.3. Quản lý tài liệu
- Danh sách, tìm kiếm, phân trang, lọc tài liệu
- Upload, download, cập nhật, xóa, chia sẻ, yêu thích tài liệu

### 3.4. Quản lý công việc (Tasks)
- Danh sách, tạo, xem chi tiết, cập nhật, xóa công việc
- Lọc theo dự án, trạng thái, ưu tiên

### 3.5. Quản lý dự án (Projects)
- Danh sách, tạo, xem chi tiết, cập nhật, xóa dự án
- Thêm/xóa thành viên, xem công việc theo dự án

### 3.6. Diễn đàn (Forum)
- Danh sách bài đăng, tạo, cập nhật, xóa, thích, bình luận, tag

### 3.7. Nhắn tin (Chat)
- Danh sách cuộc trò chuyện, nhắn tin 1-1, gửi nhận tin nhắn real-time

### 3.8. Thông báo (Notifications)
- Danh sách, đánh dấu đã đọc/xóa thông báo, thông báo real-time

### 3.9. Trang chủ (Dashboard)
- Thống kê tổng quan, biểu đồ, công việc/hoạt động gần đây

### 3.10. Cài đặt (Settings)
- Cài đặt chung, chỉnh sửa hồ sơ, cấu hình tích hợp

### 3.11. Tìm kiếm toàn cục
- Kết quả tìm kiếm toàn cục trong ứng dụng

---

## 4. Cấu trúc thư mục

### Backend (`BE/`)
```
BE/
├── src/
│   ├── config/
│   ├── controllers/
│   ├── middleware/
│   ├── models/
│   ├── routes/
│   ├── services/
│   ├── utils/
│   └── server.js
├── uploads/
├── .env
├── .env.example
├── .gitignore
├── package.json
└── README.md
```

### Frontend (`FE/`)
```
FE/
├── assets/
├── src/
│   ├── components/
│   ├── config/
│   ├── contexts/
│   ├── hooks/
│   ├── navigation/
│   ├── screens/
│   ├── services/
│   ├── styles/
│   ├── types/
│   └── utils/
├── .env
├── .env.example
├── .gitignore
├── App.tsx
├── babel.config.js
├── eas.json
├── metro.config.js
├── package.json
└── tsconfig.json
```

---

## 5. Hướng dẫn cài đặt & chạy dự án

### 5.1. Yêu cầu
- Node.js >= 18.x
- npm hoặc yarn
- MongoDB (local/Atlas)
- Expo CLI
- Máy ảo Android/iOS hoặc thiết bị thật
- Git

### 5.2. Backend (`BE/`)
1. Di chuyển vào thư mục:
    ```bash
    cd BE
    ```
2. Cài dependencies:
    ```bash
    npm install
    # hoặc
    yarn install
    ```
3. Thiết lập `.env` từ mẫu `.env.example`
4. Chạy server:
    - Development:
        ```bash
        npm run dev
        # hoặc
        yarn dev
        ```
    - Production:
        ```bash
        npm start
        # hoặc
        yarn start
        ```
    - Mặc định chạy ở `http://localhost:5001`

### 5.3. Frontend (`FE/`)
1. Di chuyển vào thư mục:
    ```bash
    cd FE
    ```
2. Cài dependencies:
    ```bash
    npm install
    # hoặc
    yarn install
    ```
3. Kiểm tra/cấu hình biến môi trường (`.env`, `api.ts`)
4. Chạy ứng dụng:
    ```bash
    npx expo start
    # hoặc
    yarn start
    ```
    - Sử dụng máy ảo, thiết bị thật hoặc Expo Go để kiểm thử
    - Nếu thiếu `@react-native-community/datetimepicker`:
        ```bash
        npx expo install @react-native-community/datetimepicker
        ```

---

## 6. Tổng quan API (Backend - `/api`)

- **/auth:** Đăng ký, đăng nhập, lấy thông tin người dùng
- **/users:** Quản lý thông tin, tìm kiếm người dùng
- **/tasks:** Danh sách, tạo, cập nhật, xóa công việc
- **/projects:** Danh sách, tạo, cập nhật, xóa dự án, quản lý thành viên
- **/documents:** Quản lý tài liệu (CRUD, chia sẻ, yêu thích)
- **/forum:** Bài đăng, bình luận, tag, thích bài đăng
- **/chats:** Danh sách chat, gửi nhận tin nhắn
- **/notifications:** Danh sách, đánh dấu đã đọc/xóa thông báo

*Chi tiết tham khảo tại `BE/README.md` hoặc source code.*

---

## 7. Tài khoản mẫu để test

- **Email:** `an.nguyen@example.com`
- **Mật khẩu:** `123456`

---

---

## 8. Liên hệ

- Đóng góp, phản hồi hoặc báo lỗi xin mở issue trên repository hoặc liên hệ qua email của nhóm phát triển.

---
