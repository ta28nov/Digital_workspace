# Dự án Không Gian Làm Việc Số - Digital Workspace Pro

## 1. Giới thiệu chung

Digital Workspace Pro là một giải pháp không gian làm việc số toàn diện, được thiết kế để nâng cao năng suất và cộng tác trong môi trường doanh nghiệp hiện đại. Dự án bao gồm một backend API mạnh mẽ và một ứng dụng di động đa nền tảng (iOS và Android) cho phép người dùng quản lý công việc, tài liệu, dự án, giao tiếp và cập nhật thông tin một cách hiệu quả.

Ứng dụng hướng tới việc cung cấp một giao diện người dùng trực quan, thân thiện, hỗ trợ các tính năng cần thiết cho công việc hàng ngày và quản lý đội nhóm.

## 2. Công nghệ sử dụng

### Backend (Thư mục `BE`)

*   **Ngôn ngữ**: JavaScript (Node.js)
*   **Framework**: Express.js
*   **Cơ sở dữ liệu**: MongoDB (sử dụng Mongoose ODM)
*   **Xác thực**: JSON Web Tokens (JWT)
*   **Upload file**: Multer (lưu trữ cục bộ trong thư mục `uploads`)
*   **Real-time**: Socket.IO (cho thông báo và chat)
*   **Logging**: Winston (được cấu hình trong `utils/logger.js`)
*   **Khác**: `bcryptjs` (băm mật khẩu), `dotenv` (quản lý biến môi trường), `express-async-handler` (xử lý lỗi bất đồng bộ), `cors` (quản lý Cross-Origin Resource Sharing).

### Frontend (Thư mục `FE`)

*   **Ngôn ngữ**: TypeScript
*   **Framework**: React Native (sử dụng Expo)
*   **Quản lý state**: React Context API, React Hooks (`useState`, `useEffect`, `useContext`)
*   **Điều hướng**: React Navigation (`@react-navigation/native`, `@react-navigation/stack`, `@react-navigation/bottom-tabs`)
*   **UI Components**: React Native Paper (cho nhiều thành phần UI), `@react-native-community/datetimepicker`
*   **Icons**: `@expo/vector-icons` (MaterialCommunityIcons)
*   **API Client**: Axios (được cấu hình trong `services/api.ts`)
*   **Xử lý font**: `expo-font`
*   **Khác**: `expo-secure-store` (lưu trữ token an toàn), `date-fns` (thao tác ngày giờ).

## 3. Tính năng chính

### 3.1. Xác thực người dùng (Authentication)

*   **Đăng ký**: Người dùng có thể tạo tài khoản mới với tên, email và mật khẩu. Mật khẩu được băm an toàn.
*   **Đăng nhập**: Người dùng đăng nhập bằng email và mật khẩu, nhận về JWT token để truy cập các tài nguyên được bảo vệ.
*   **Lấy thông tin cá nhân (`/auth/me`)**: Lấy thông tin người dùng đang đăng nhập.
*   **Quên mật khẩu (FE)**: Chức năng khôi phục mật khẩu (chi tiết triển khai frontend cần xem thêm).
*   **Màn hình chờ (Splash Screen)**: Hiển thị khi khởi động ứng dụng.
*   **Onboarding**: Hướng dẫn người dùng mới qua các bước giới thiệu ứng dụng.

### 3.2. Quản lý người dùng (Users)

*   **Xem thông tin cá nhân (`/users/me`)**: Xem chi tiết thông tin cá nhân của người dùng đang đăng nhập.
*   **Cập nhật thông tin cá nhân (`/users/me`)**: Cập nhật tên, avatar, bio, và cài đặt riêng tư (hiển thị email, trạng thái hoạt động).
*   **Tìm kiếm người dùng (`/users/search`)**: Tìm kiếm người dùng khác theo tên hoặc email.
*   **Xem thông tin người dùng khác (`/users/:id`)**: Xem thông tin công khai của người dùng khác, có kiểm tra cài đặt riêng tư.

### 3.3. Quản lý Tài liệu (Documents)

*   **Danh sách tài liệu (`/documents`)**:
    *   Xem danh sách các tài liệu mà người dùng tạo hoặc được chia sẻ.
    *   Lọc theo dự án, loại tài liệu, tag, trạng thái chia sẻ, yêu thích.
    *   Tìm kiếm theo tiêu đề, mô tả, tag.
    *   Sắp xếp theo các trường khác nhau (ví dụ: `updatedAt`).
    *   Phân trang.
*   **Tải lên tài liệu mới (`/documents`)**:
    *   Upload file (hình ảnh, pdf, docx, v.v.) kèm theo tiêu đề, mô tả, tags, loại tài liệu, và (tùy chọn) dự án liên quan.
    *   Hệ thống tự động suy luận loại tài liệu từ phần mở rộng của file nếu không được cung cấp.
    *   File được lưu trữ trên server (thư mục `uploads`).
*   **Xem chi tiết tài liệu (`/documents/:id`)**:
    *   Xem metadata chi tiết của tài liệu (tiêu đề, mô tả, người tạo, ngày tạo, tags, thông tin file).
    *   Hiển thị trạng thái yêu thích.
    *   Populate thông tin người tạo và người được chia sẻ.
*   **Tải xuống tài liệu (`/documents/:id/download`)**:
    *   Cung cấp URL để tải file gốc của tài liệu.
*   **Cập nhật metadata tài liệu (`/documents/:id`)**:
    *   Chủ sở hữu có thể cập nhật tiêu đề, mô tả, tags của tài liệu.
*   **Xóa tài liệu (`/documents/:id`)**:
    *   Chủ sở hữu có thể xóa tài liệu (bao gồm cả file vật lý trên server và các bản ghi yêu thích liên quan).
*   **Chia sẻ tài liệu (`/documents/:id/share`)**:
    *   Chủ sở hữu có thể chia sẻ tài liệu với người dùng khác với quyền đọc (`read`) hoặc chỉnh sửa (`edit`).
    *   Thông báo được gửi đến người dùng được chia sẻ.
*   **Yêu thích/Bỏ yêu thích tài liệu (`/documents/:id/favorite`)**:
    *   Người dùng có thể đánh dấu hoặc bỏ đánh dấu yêu thích một tài liệu mà họ có quyền truy cập.
*   **Giao diện người dùng (FE)**:
    *   `DocumentScreen`: Hiển thị danh sách tài liệu, cho phép lọc, tìm kiếm, tạo mới, và điều hướng đến chi tiết.
    *   `DocumentDetailScreen`: Hiển thị chi tiết tài liệu, cho phép tải xuống, chia sẻ (dự kiến), yêu thích.

### 3.4. Quản lý Công việc (Tasks)

*   **Danh sách công việc (`/tasks`)**:
    *   Xem danh sách công việc của người dùng.
    *   Lọc theo dự án (có kiểm tra quyền thành viên), trạng thái hoàn thành, mức độ ưu tiên.
    *   Sắp xếp và phân trang.
*   **Tạo công việc mới (`/tasks`)**:
    *   Tạo công việc với tiêu đề, mức độ ưu tiên, ngày hết hạn (tùy chọn), và dự án liên quan (tùy chọn, có kiểm tra quyền thành viên).
*   **Xem chi tiết công việc (`/tasks/:id`)**:
    *   Người dùng có thể xem chi tiết công việc của mình.
*   **Cập nhật công việc (`/tasks/:id`)**:
    *   Người dùng có thể cập nhật tiêu đề, trạng thái hoàn thành, mức độ ưu tiên, ngày hết hạn của công việc.
*   **Xóa công việc (`/tasks/:id`)**:
    *   Người dùng có thể xóa công việc của mình.
*   **Giao diện người dùng (FE)**:
    *   `TaskManagementScreen`: Hiển thị danh sách công việc, cho phép tạo mới (qua modal), lọc, đánh dấu hoàn thành, và điều hướng đến chi tiết. Sử dụng `@react-native-community/datetimepicker` để chọn ngày.
    *   `TaskDetailScreen`: Hiển thị chi tiết công việc, cho phép cập nhật trạng thái hoàn thành và xóa công việc.

### 3.5. Quản lý Dự án (Projects)

*   **Danh sách dự án (`/projects`)**:
    *   Xem danh sách các dự án mà người dùng là thành viên.
    *   Lọc theo trạng thái (active, archived).
    *   Sắp xếp và phân trang.
*   **Tạo dự án mới (`/projects`)**:
    *   Tạo dự án với tên, mô tả. Người tạo tự động là chủ sở hữu và thành viên.
    *   Có thể thêm các thành viên khác khi tạo dự án.
*   **Xem chi tiết dự án (`/projects/:id`)**:
    *   Thành viên dự án có thể xem thông tin chi tiết (tên, mô tả, chủ sở hữu, danh sách thành viên).
*   **Cập nhật dự án (`/projects/:id`)**:
    *   Chủ sở hữu dự án có thể cập nhật tên, mô tả, trạng thái.
*   **Xóa dự án (`/projects/:id`)**:
    *   Chủ sở hữu có thể xóa dự án. (Cần xem xét việc xử lý các tác vụ, tài liệu liên quan).
*   **Quản lý thành viên dự án**:
    *   **Thêm thành viên (`/projects/:id/members`)**: Chủ sở hữu có thể thêm người dùng khác vào dự án.
    *   **Xóa thành viên (`/projects/:id/members/:userId`)**: Chủ sở hữu có thể xóa thành viên khỏi dự án (không thể xóa chính mình).
*   **Xem công việc của dự án (`/projects/:id/tasks`)**:
    *   Thành viên dự án có thể xem danh sách công việc thuộc dự án đó, với các tùy chọn lọc và sắp xếp tương tự như `/api/tasks`.
*   **Giao diện người dùng (FE)**:
    *   `ProjectsScreen` (dự kiến, chưa thấy file cụ thể nhưng có thư mục `Projects`): Quản lý dự án.
    *   `ResourceViewScreen` (trong `Projects`): Có thể liên quan đến việc hiển thị tài nguyên của dự án.

### 3.6. Diễn đàn (Forum)

*   **Danh sách bài đăng (`/forum/posts`)**:
    *   Xem danh sách các bài đăng trên diễn đàn.
    *   Lọc theo tag.
    *   Sắp xếp và phân trang.
    *   Hiển thị trạng thái `isLiked` nếu người dùng đã đăng nhập.
*   **Tạo bài đăng mới (`/forum/posts`)**:
    *   Người dùng đã đăng nhập có thể tạo bài đăng với tiêu đề, nội dung và tags.
*   **Xem chi tiết bài đăng (`/forum/posts/:postId`)**:
    *   Xem chi tiết nội dung bài đăng, thông tin tác giả.
    *   Hiển thị trạng thái `isLiked` nếu người dùng đã đăng nhập.
*   **Cập nhật bài đăng (`/forum/posts/:postId`)**:
    *   Tác giả bài đăng có thể cập nhật tiêu đề, nội dung, tags.
*   **Xóa bài đăng (`/forum/posts/:postId`)**:
    *   Tác giả bài đăng có thể xóa bài đăng (bao gồm cả bình luận và lượt thích liên quan).
*   **Thích/Bỏ thích bài đăng (`/forum/posts/:postId/like`)**:
    *   Người dùng đã đăng nhập có thể thích hoặc bỏ thích một bài đăng. Số lượt thích (`likesCount`) được cập nhật.
*   **Quản lý bình luận**:
    *   **Xem bình luận (`/forum/posts/:postId/comments`)**: Xem danh sách bình luận của một bài đăng, có phân trang.
    *   **Tạo bình luận mới (`/forum/posts/:postId/comments`)**: Người dùng đã đăng nhập có thể bình luận về một bài đăng. Số bình luận (`commentsCount`) trên bài đăng được cập nhật. Thông báo được gửi cho tác giả bài đăng.
    *   **Xóa bình luận (`/forum/comments/:commentId`)**: Tác giả bình luận có thể xóa bình luận của mình. Số bình luận trên bài đăng được cập nhật.
*   **Lấy danh sách Tags (`/forum/tags`)**:
    *   Lấy danh sách các tag duy nhất đã được sử dụng trên diễn đàn.
*   **Giao diện người dùng (FE)**:
    *   `ForumScreen`: Hiển thị danh sách bài đăng, lọc theo tag, điều hướng đến chi tiết hoặc tạo bài mới.
    *   `ForumPostDetailScreen`: Hiển thị chi tiết bài đăng, bình luận, cho phép thích, bình luận.
    *   `CreateForumPostScreen`: Form tạo bài đăng mới.

### 3.7. Nhắn tin (Chat)

*   **Danh sách cuộc trò chuyện (`/chats`)**:
    *   Xem danh sách các cuộc trò chuyện mà người dùng tham gia.
    *   Hiển thị người tham gia khác, tin nhắn cuối cùng, số tin nhắn chưa đọc.
    *   Sắp xếp theo thời gian cập nhật cuối cùng, phân trang.
*   **Tạo hoặc lấy cuộc trò chuyện 1-1 (`/chats`)**:
    *   Nếu cuộc trò chuyện với người dùng khác đã tồn tại, trả về thông tin cuộc trò chuyện đó.
    *   Nếu chưa, tạo một cuộc trò chuyện mới giữa hai người dùng.
*   **Xem tin nhắn trong cuộc trò chuyện (`/chats/:chatId/messages`)**:
    *   Lấy danh sách tin nhắn của một cuộc trò chuyện cụ thể, có phân trang (tải tin nhắn cũ hơn).
    *   Tin nhắn được đánh dấu là đã đọc khi người dùng xem.
*   **Gửi tin nhắn (`/chats/:chatId/messages`)**:
    *   Gửi tin nhắn văn bản trong một cuộc trò chuyện.
    *   Cập nhật tin nhắn cuối cùng và thời gian cập nhật của cuộc trò chuyện.
    *   Thông báo được gửi đến người nhận tin nhắn.
    *   Tin nhắn được gửi qua Socket.IO để cập nhật real-time (cần kiểm tra chi tiết triển khai).
*   **Giao diện người dùng (FE)**:
    *   `ChatListScreen`: Hiển thị danh sách các cuộc trò chuyện, cho phép tạo cuộc trò chuyện mới (điều hướng đến `CreateChatUserSelectionScreen`).
    *   `CreateChatUserSelectionScreen`: Màn hình chọn người dùng để bắt đầu cuộc trò chuyện mới.
    *   `ChatDetailScreen`: Hiển thị nội dung cuộc trò chuyện, cho phép gửi tin nhắn.

### 3.8. Thông báo (Notifications)

*   **Danh sách thông báo (`/notifications`)**:
    *   Xem danh sách thông báo của người dùng.
    *   Lọc theo trạng thái đọc/chưa đọc, loại thông báo.
    *   Sắp xếp mới nhất trước, phân trang.
    *   Trả về tổng số thông báo và số thông báo chưa đọc.
*   **Đánh dấu đã đọc**:
    *   **Đánh dấu một số thông báo đã đọc (`/notifications/read`)**: Cập nhật trạng thái của các thông báo cụ thể thành đã đọc.
    *   **Đánh dấu tất cả đã đọc (`/notifications/read-all`)**: Cập nhật trạng thái của tất cả thông báo chưa đọc thành đã đọc.
*   **Xóa thông báo**:
    *   **Xóa một thông báo cụ thể (`/notifications/:id`)**: Xóa một thông báo.
    *   **Xóa tất cả thông báo (`/notifications`)**: Xóa tất cả thông báo của người dùng.
*   **Thông báo Real-time**: Hệ thống sử dụng Socket.IO để gửi thông báo mới đến client trong thời gian thực.
*   **Các loại thông báo được tạo tự động**:
    *   Chia sẻ tài liệu.
    *   Bình luận mới trên bài đăng diễn đàn.
    *   Tin nhắn mới.
    *   (Có thể có các loại khác như được thêm vào dự án, được giao task - cần xem xét thêm).
*   **Giao diện người dùng (FE)**:
    *   `NotificationScreen`: Hiển thị danh sách thông báo, cho phép lọc và có thể có các hành động đánh dấu đã đọc/xóa.

### 3.9. Trang chủ (Dashboard - FE)

*   `HomeScreen`: Màn hình chính sau khi đăng nhập, thường hiển thị:
    *   Tổng quan thống kê (công việc, tài liệu).
    *   Biểu đồ (tiến độ, hoạt động).
    *   Thông tin thời tiết (dự kiến).
    *   Danh sách công việc/hoạt động gần đây.

### 3.10. Cài đặt (Settings - FE)

*   `SettingsScreen`: Màn hình cài đặt chung.
*   `ProfileScreen`: Xem và chỉnh sửa thông tin cá nhân.
*   `PendingSyncScreen`: Có thể liên quan đến đồng bộ hóa dữ liệu offline (cần xem xét thêm).
*   `IntegrationSettingsScreen`: Có thể liên quan đến tích hợp với các dịch vụ khác.

### 3.11. Tìm kiếm (Search - FE)

*   `GlobalSearchResultsScreen`: Hiển thị kết quả tìm kiếm toàn cục trong ứng dụng.

## 4. Cấu trúc dự án

### 4.1. Backend (`BE/`)

```
BE/
├── src/
│   ├── config/       # Cấu hình (db.js, passport-config.js nếu có)
│   ├── controllers/  # Logic xử lý request/response (authController.js, userController.js, ...)
│   ├── middleware/   # Middleware (authMiddleware.js, errorMiddleware.js, uploadMiddleware.js)
│   ├── models/       # Mongoose models (User.js, Task.js, Project.js, ...)
│   ├── routes/       # Định nghĩa các routes API (authRoutes.js, userRoutes.js, ...)
│   ├── services/     # (Tùy chọn) Logic nghiệp vụ phức tạp
│   ├── utils/        # Các hàm tiện ích (generateToken.js, logger.js)
│   └── server.js     # Entry point của ứng dụng, khởi tạo Express và Socket.IO
├── uploads/          # Thư mục lưu trữ file upload (được tạo tự động, nên thêm vào .gitignore)
├── .env              # Biến môi trường (cần tạo thủ công từ .env.example)
├── .env.example      # Mẫu biến môi trường
├── .gitignore        # Các file/thư mục bị bỏ qua bởi Git
├── package.json      # Metadata và dependencies
└── README.md         # Tài liệu hướng dẫn backend (hiện tại)
```

### 4.2. Frontend (`FE/`)

```
FE/
├── assets/             # Tài nguyên tĩnh (hình ảnh, font chữ)
├── src/
│   ├── components/     # Các component có thể tái sử dụng
│   │   ├── common/     # Component chung
│   │   └── ...         # Component theo tính năng
│   ├── config/         # Cấu hình ứng dụng (ví dụ: apiConfig.ts)
│   ├── contexts/       # React Context (AuthContext, ThemeContext, ...)
│   ├── hooks/          # Custom React Hooks
│   ├── navigation/     # Cấu hình React Navigation (navigators, types)
│   ├── screens/        # Các màn hình của ứng dụng
│   │   ├── Auth/       # Màn hình xác thực (Login, Register, SplashScreen)
│   │   ├── Main/       # Màn hình chính sau đăng nhập (Home, Document, Task, Chat, Forum, Settings, ...)
│   │   │   ├── Projects/
│   │   │   ├── Search/
│   │   │   └── Settings/
│   │   └── Onboarding/ # Màn hình giới thiệu ứng dụng
│   ├── services/       # Logic gọi API (api.ts)
│   ├── styles/         # Kiểu dáng và theme toàn cục
│   ├── types/          # Định nghĩa TypeScript (interfaces, types)
│   └── utils/          # Các tiện ích và hàm hỗ trợ (constants.ts, helpers.ts)
├── .env                # Biến môi trường cho frontend (nếu có, ví dụ API_URL)
├── .env.example        # Mẫu biến môi trường frontend
├── .gitignore
├── App.tsx             # Entry point của ứng dụng React Native
├── babel.config.js
├── eas.json            # Cấu hình Expo Application Services (EAS)
├── metro.config.js
├── package.json
└── tsconfig.json       # Cấu hình TypeScript
```

## 5. Hướng dẫn cài đặt và chạy dự án

### 5.1. Yêu cầu chung

*   [Node.js](https://nodejs.org/) (khuyến nghị phiên bản LTS mới nhất, ví dụ >= 18.x)
*   [npm](https://www.npmjs.com/) hoặc [yarn](https://yarnpkg.com/)
*   [MongoDB](https://www.mongodb.com/) (cài đặt local hoặc sử dụng Atlas cluster)
*   [Expo CLI](https://docs.expo.dev/get-started/installation/) (cho frontend)
*   Máy ảo Android/iOS hoặc thiết bị thật để chạy ứng dụng di động.
*   Công cụ dòng lệnh Git.

### 5.2. Backend (`BE/`)

1.  **Di chuyển vào thư mục `BE`**:
    ```bash
    cd BE
    ```

2.  **Cài đặt dependencies**:
    ```bash
    npm install
    # hoặc
    yarn install
    ```

3.  **Thiết lập biến môi trường**:
    *   Tạo một file `.env` trong thư mục gốc `BE`.
    *   Sao chép nội dung từ `BE/.env.example` (nếu có, hoặc tạo mới dựa trên ví dụ sau) vào `.env`:
        ```dotenv
        PORT=5001
        MONGODB_URI=mongodb://localhost:27017/digital_workspace_pro_db
        JWT_SECRET=your_super_secret_jwt_key_change_this_!@#
        LOG_LEVEL=info

        # Cấu hình Socket.IO (nếu cần)
        # SOCKET_CORS_ORIGIN=http://localhost:8081
        ```
    *   **Lưu ý**:
        *   Thay `MONGODB_URI` nếu bạn dùng MongoDB Atlas hoặc cấu hình khác.
        *   Thay `JWT_SECRET` bằng một chuỗi bí mật mạnh và duy nhất.
        *   `SOCKET_CORS_ORIGIN` nên trỏ đến địa chỉ của ứng dụng frontend React Native khi chạy (thường là `http://localhost:8081` hoặc địa chỉ IP của máy phát triển).

4.  **Chạy Backend Server**:
    *   **Chế độ Development (với nodemon, tự động restart khi có thay đổi code)**:
        ```bash
        npm run dev
        # hoặc
        yarn dev
        ```
    *   **Chế độ Production**:
        ```bash
        npm start
        # hoặc
        yarn start
        ```
    Server sẽ chạy trên cổng được định nghĩa trong `.env` (ví dụ: `http://localhost:5001`).

### 5.3. Frontend (`FE/`)

1.  **Di chuyển vào thư mục `FE`**:
    ```bash
    cd FE
    ```

2.  **Cài đặt dependencies**:
    ```bash
    npm install
    # hoặc
    yarn install
    ```
    *Lưu ý: Nếu gặp lỗi liên quan đến `peerDependencies`, thử thêm cờ `--legacy-peer-deps` (cho npm) hoặc giải quyết các xung đột theo hướng dẫn.*

3.  **Thiết lập biến môi trường (nếu cần)**:
    *   Kiểm tra xem có file `.env` hoặc `.env.example` trong thư vực `FE` không. Nếu có, cấu hình tương tự backend.
    *   Thông thường, địa chỉ API backend sẽ được cấu hình trong `FE/src/services/api.ts` hoặc một file config tương tự. Đảm bảo nó trỏ đúng đến địa chỉ backend server đang chạy (ví dụ: `http://localhost:5001/api` hoặc `http://<your-ip-address>:5001/api` nếu chạy trên thiết bị thật).

4.  **Chạy ứng dụng Frontend (Expo)**:
    *   **Khởi động Metro Bundler**:
        ```bash
        npx expo start
        # hoặc
        yarn start
        ```
    *   Sau đó, một trang web sẽ mở ra trong trình duyệt. Bạn có thể:
        *   Nhấn `a` để mở trên máy ảo Android (cần Android Studio và máy ảo đã được cài đặt và khởi chạy).
        *   Nhấn `i` để mở trên máy ảo iOS (chỉ dành cho macOS, cần Xcode và máy ảo đã cài đặt).
        *   Quét mã QR bằng ứng dụng Expo Go trên thiết bị di động thật (cùng mạng Wi-Fi với máy phát triển).

    *   **Cài đặt `@react-native-community/datetimepicker` (nếu chưa có)**:
        Nếu gặp lỗi module not found cho `datetimepicker`, hãy cài đặt nó:
        ```bash
        npx expo install @react-native-community/datetimepicker
        ```

## 6. Tổng quan API (Backend - `/api`)

Dưới đây là tóm tắt các nhóm API chính. Tham khảo file `BE/README.md` hoặc code trong `BE/src/controllers/` và `BE/src/routes/` để biết chi tiết đầy đủ về request body, response format, và các mã lỗi.

*   **Authentication (`/auth`)**:
    *   `POST /register`: Đăng ký người dùng mới.
    *   `POST /login`: Đăng nhập, nhận token.
    *   `GET /me`: Lấy thông tin người dùng hiện tại (yêu cầu token).

*   **Users (`/users`)**:
    *   `GET /me`: Lấy thông tin người dùng hiện tại.
    *   `PUT /me`: Cập nhật thông tin người dùng hiện tại.
    *   `GET /search`: Tìm kiếm người dùng theo tên/email.
    *   `GET /:id`: Lấy thông tin công khai của người dùng khác.

*   **Tasks (`/tasks`)**:
    *   `GET /`: Lấy danh sách tác vụ (filter, pagination).
    *   `POST /`: Tạo tác vụ mới.
    *   `GET /:id`: Lấy chi tiết tác vụ.
    *   `PUT /:id`: Cập nhật tác vụ.
    *   `DELETE /:id`: Xóa tác vụ.

*   **Projects (`/projects`)**:
    *   `GET /`: Lấy danh sách dự án người dùng là thành viên.
    *   `POST /`: Tạo dự án mới.
    *   `GET /:id`: Lấy chi tiết dự án.
    *   `PUT /:id`: Cập nhật dự án.
    *   `DELETE /:id`: Xóa dự án.
    *   `POST /:id/members`: Thêm thành viên vào dự án.
    *   `DELETE /:id/members/:userId`: Xóa thành viên khỏi dự án.
    *   `GET /:id/tasks`: Lấy danh sách tác vụ của dự án.

*   **Documents (`/documents`)**:
    *   `GET /`: Lấy danh sách tài liệu (filter, pagination, search).
    *   `POST /`: Upload tài liệu mới (sử dụng `multipart/form-data`).
    *   `GET /:id`: Lấy metadata tài liệu.
    *   `GET /:id/download`: Lấy URL download file.
    *   `PUT /:id`: Cập nhật metadata tài liệu.
    *   `DELETE /:id`: Xóa tài liệu.
    *   `POST /:id/share`: Chia sẻ tài liệu.
    *   `POST /:id/favorite`: Yêu thích/bỏ yêu thích tài liệu.

*   **Forum (`/forum`)**:
    *   `GET /posts`: Lấy danh sách bài đăng (filter, pagination).
    *   `POST /posts`: Tạo bài đăng mới.
    *   `GET /posts/:postId`: Lấy chi tiết bài đăng.
    *   `PUT /posts/:postId`: Cập nhật bài đăng.
    *   `DELETE /posts/:postId`: Xóa bài đăng.
    *   `POST /posts/:postId/like`: Like/unlike bài đăng.
    *   `GET /posts/:postId/comments`: Lấy bình luận của bài đăng.
    *   `POST /posts/:postId/comments`: Tạo bình luận mới.
    *   `DELETE /comments/:commentId`: Xóa bình luận.
    *   `GET /tags`: Lấy danh sách tags.

*   **Chats (`/chats`)**:
    *   `GET /`: Lấy danh sách chat của người dùng.
    *   `POST /`: Tạo hoặc lấy chat 1-1 với người dùng khác.
    *   `GET /:chatId/messages`: Lấy tin nhắn trong chat (pagination).
    *   `POST /:chatId/messages`: Gửi tin nhắn.

*   **Notifications (`/notifications`)**:
    *   `GET /`: Lấy danh sách thông báo (filter, pagination).
    *   `POST /read`: Đánh dấu các thông báo đã đọc.
    *   `POST /read-all`: Đánh dấu tất cả đã đọc.
    *   `DELETE /`: Xóa tất cả thông báo.
    *   `DELETE /:id`: Xóa thông báo cụ thể.

## 7. Tài khoản mẫu để test

*   **Email**: `an.nguyen@example.com`
*   **Mật khẩu**: `123456`

(Hoặc các tài khoản khác bạn đã tạo trong quá trình phát triển/seed data.)

## 8. Hướng phát triển và Cải tiến tiềm năng

*   **Backend**:
    *   Triển khai phân quyền chi tiết hơn (RBAC).
    *   Hoàn thiện xử lý xóa dữ liệu liên quan (ví dụ, khi xóa dự án, xóa các task, document liên quan hoặc cho phép hủy liên kết).
    *   Tối ưu hóa các truy vấn cơ sở dữ liệu phức tạp.
    *   Thêm unit tests và integration tests.
    *   Sử dụng một dịch vụ lưu trữ file trên cloud (S3, Firebase Storage) thay vì lưu cục bộ cho production.
    *   Cải thiện tài liệu API (ví dụ: Swagger/OpenAPI).
*   **Frontend**:
    *   Hoàn thiện các màn hình còn thiếu hoặc đang phát triển (ví dụ: chi tiết dự án, quản lý thành viên dự án đầy đủ từ FE).
    *   Triển khai đầy đủ các tính năng đã được thiết kế (ví dụ: thời tiết, các loại biểu đồ dashboard).
    *   Xử lý offline và đồng bộ hóa dữ liệu.
    *   Tối ưu hóa hiệu năng, đặc biệt với danh sách dài và hình ảnh.
    *   Thêm test cho components và business logic.
    *   Cải thiện UI/UX dựa trên feedback.
*   **Chung**:
    *   Triển khai CI/CD pipeline.
    *   Giám sát và logging nâng cao cho production.
