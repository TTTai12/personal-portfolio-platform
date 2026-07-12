# 🚀 HƯỚNG DẪN DEPLOY DỰ ÁN LÊN RENDER (FRONTEND & BACKEND)

Dự án của bạn gồm 2 phần trong cùng một repository:
1. **Frontend (Vite/React)**: Nằm ở thư mục gốc (`/`). Sẽ được deploy dưới dạng **Static Site** (Miễn phí, luôn hoạt động, không bị ngủ đông).
2. **Backend (Next.js/Admin-Backend)**: Nằm ở thư mục con (`/admin-backend`). Sẽ được deploy dưới dạng **Web Service** (Chạy server Node.js, có thể bị ngủ đông sau 15 phút không hoạt động ở gói Free).

---

## 📌 ĐIỀU KIỆN TIÊN QUYẾT
* Bạn đã push toàn bộ code mới nhất lên GitHub. Repo hiện tại là: `https://github.com/TTTai12/My_Portfolio.git` (nhánh `main`).
* Đăng nhập tài khoản **[Render](https://render.com/)** bằng tài khoản GitHub của bạn.

---

## 💥 BƯỚC 1: DEPLOY BACKEND (Next.js - admin-backend)
Vì Next.js cần xử lý API, kết nối Database (MongoDB) và xử lý xác thực (NextAuth), chúng ta phải deploy nó dưới dạng **Web Service**.

### 1️⃣ Tạo Web Service Mới
1. Trên dashboard Render, click **New +** → **Web Service**.
2. Chọn **Build and deploy from a Git repository** → Click **Next**.
3. Chọn repository **`My_Portfolio`** từ danh sách kết nối GitHub của bạn.

### 2️⃣ Cấu Hình Thông Tin Cơ Bản
* **Name**: `admin-portfolio-backend` (hoặc tên bất kỳ bạn thích).
* **Region**: Chọn **Singapore (ap-southeast-1)** để có tốc độ kết nối về Việt Nam tốt nhất.
* **Branch**: `main`.
* **Root Directory**: `admin-backend` ⚠️ *(Rất quan trọng, để chỉ định deploy thư mục con)*.
* **Runtime**: `Node`.
* **Build Command**: `npm run build`
* **Start Command**: `npm run start`

> [!TIP]
> **Khắc phục lỗi thiếu bộ nhớ (Out of Memory) trên Render Free Tier:**
> Gói Free của Render chỉ có 512MB RAM. Tiến trình build mặc định của Next.js rất nặng và dễ làm crash quá trình deploy. 
> Để khắc phục, bạn có thể thiết lập **Build Command** thành:
> ```bash
> NODE_OPTIONS='--max-old-space-size=450' npm run build
> ```

### 3️⃣ Cấu Hình Environment Variables (Biến môi trường)
Click vào mục **Advanced** ở dưới cùng để thêm các biến môi trường từ file `.env.local` của backend:

| Key | Value | Mô tả / Lưu ý |
|-----|-------|---------------|
| `MONGODB_URI` | `mongodb+srv://tientantai12_db_user:xsTjCsDZYqfU84ne@myportfoliocluster.3ep6jh6.mongodb.net/` | URL MongoDB Atlas của bạn |
| `NEXTAUTH_SECRET` | `+vSh4uq7MuUHgQLhJH6edd+t7kQA1Y+y2GujRN0bonU=` | Khóa bảo mật NextAuth |
| `NEXTAUTH_URL` | `https://admin-portfolio-backend.onrender.com` | **Thay bằng URL Backend Render thực tế của bạn** sau khi tạo dịch vụ xong |
| `ADMIN_USERNAME` | `admin` | Tài khoản admin |
| `ADMIN_PASSWORD` | `admin123` | Mật khẩu admin |
| `EMAIL_USER` | `tientantai12@gmail.com` | Email gửi tin nhắn |
| `EMAIL_PASS` | `lwfwhqkqrazpyiib` | Mật khẩu ứng dụng Gmail |
| `EMAIL_TO` | `tientantai12@gmail.com` | Email nhận thông báo |
| `EMAIL_HOST` | `smtp.gmail.com` | SMTP Host |
| `EMAIL_PORT` | `465` | SMTP Port |
| `EMAIL_SECURE` | `true` | Sử dụng SSL/TLS |
| `NEXT_PUBLIC_CLOUDINARY_CLOUD_NAME` | `dvfbzhr11` | Cloudinary Cloud Name |
| `CORS_ORIGIN` | `*` | *(Tạm thời để `*`, chúng ta sẽ cập nhật lại sau khi có URL Frontend)* |

4. Cuộn xuống và chọn gói **Free** → Click **Create Web Service**.

> [!NOTE]
> Hệ thống sẽ tiến hành build và deploy. Sau khi thành công, Render sẽ hiển thị URL backend ở góc trên bên trái (ví dụ: `https://admin-portfolio-backend.onrender.com`).
> **Hãy copy URL này để cấu hình cho Frontend.**

---

## 🌐 BƯỚC 2: DEPLOY FRONTEND (Vite - my-portfolio)
Frontend là ứng dụng Single Page App tĩnh, chúng ta deploy dưới dạng **Static Site** để tối ưu hóa tốc độ và hoàn toàn miễn phí, không bao giờ bị ngủ đông.

### 1️⃣ Tạo Static Site Mới
1. Trên dashboard Render, click **New +** → **Static Site**.
2. Chọn repository **`My_Portfolio`**.

### 2️⃣ Cấu Hình Thông Tin Cơ Bản
* **Name**: `my-portfolio-frontend` (hoặc tên bất kỳ bạn thích).
* **Branch**: `main`.
* **Root Directory**: Để trống hoặc điền `.` (vì code frontend nằm ở thư mục gốc của repo).
* **Build Command**: `npm run build`
* **Publish Directory**: `dist`

### 3️⃣ Cấu Hình Environment Variables cho Frontend
Tại mục **Environment Variables**, click **Add Environment Variable** để thêm 2 biến liên kết với Backend:

| Key | Value |
|-----|-------|
| `VITE_API_URL` | `https://admin-portfolio-backend.onrender.com/api` |
| `VITE_ADMIN_URL` | `https://admin-portfolio-backend.onrender.com/auth/login` |

> ⚠️ *Lưu ý: Thay `https://admin-portfolio-backend.onrender.com` bằng URL thực tế của backend bạn vừa deploy ở Bước 1.*

4. Click **Create Static Site**.

> [!NOTE]
> Render sẽ build frontend Vite và xuất ra thư mục `dist`. Khi hoàn tất, bạn sẽ nhận được URL Frontend của mình (ví dụ: `https://my-portfolio-frontend.onrender.com`).

---

## 🔒 BƯỚC 3: CẬP NHẬT CORS CHO BACKEND (Rất quan trọng)
Để bảo mật và cho phép Frontend gửi request API thành công tới Backend mà không bị lỗi CORS:

1. Vào Dashboard Render → Chọn dịch vụ Backend **`admin-portfolio-backend`**.
2. Đi tới tab **Environment** (bên trái).
3. Tìm biến **`CORS_ORIGIN`** và cập nhật giá trị của nó từ `*` thành URL Frontend của bạn:
   * Ví dụ: `https://my-portfolio-frontend.onrender.com`
4. Tìm biến **`NEXTAUTH_URL`** và cập nhật chính xác URL Backend của bạn nếu ban đầu bạn nhập tạm thời.
5. Click **Save Changes**.

Render sẽ tự động redeploy lại Backend để áp dụng các thay đổi biến môi trường mới.

---

## 🔍 BƯỚC 4: KIỂM TRA SAU KHI DEPLOY
1. Truy cập vào link Frontend trên trình duyệt.
2. Nhấn **F12** → Chọn tab **Network**.
3. Reload lại trang và kiểm tra xem các request API gửi tới backend (ví dụ `/api/projects`) có nhận được phản hồi thành công (Status Code `200`) hay không.
4. Truy cập vào trang admin (`/auth/login`) và đăng nhập thử để kiểm tra tính năng kết nối Database và Auth.

---

## 💡 LƯU Ý KHI SỬ DỤNG GÓI FREE CỦA RENDER
* **Tự động ngủ đông**: Web Service (Backend) sẽ tự động tắt nếu không có request nào trong vòng 15 phút. Khi có lượt truy cập mới, server sẽ mất khoảng **50 - 90 giây** để khởi động lại (Cold Start). Điều này làm cho lượt load đầu tiên bị chậm.
* **Thời gian Build giới hạn**: Tài khoản Render Free có giới hạn số phút build miễn phí mỗi tháng (thường là 500 phút). Hãy hạn chế commit/deploy liên tục không cần thiết để tránh hết hạn mức.
