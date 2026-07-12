# 🚀 HƯỚNG DẪN DEPLOY BACKEND LÊN RENDER (CHO DỰ ÁN MY-PORTFOLIO)

Vì phần Frontend (Vite) của dự án **my-portfolio** đã được deploy thành công trên **Vercel**, bạn chỉ cần deploy phần Backend quản trị (**admin-backend** - Next.js) lên **Render**, sau đó kết nối hai bên với nhau.

---

## 📌 ĐIỀU KIỆN TIÊN QUYẾT
* Đã push code mới nhất lên GitHub của repository: `https://github.com/TTTai12/My_Portfolio.git` (nhánh `main`).
* Đăng nhập tài khoản **[Render](https://render.com/)** bằng tài khoản GitHub của bạn.

---

## 💥 BƯỚC 1: DEPLOY BACKEND (`admin-backend` folder) LÊN RENDER

### 1️⃣ Tạo Web Service Mới
1. Trên trang Dashboard của Render, chọn **New +** → **Web Service**.
2. Chọn **Build and deploy from a Git repository** → Click **Next**.
3. Chọn repository **`My_Portfolio`** từ danh sách kết nối GitHub của bạn.

### 2️⃣ Cấu Hình Thông Tin Cơ Bản
* **Name**: `admin-portfolio-backend` (hoặc tên bất kỳ bạn thích).
* **Region**: Chọn **Singapore (ap-southeast-1)** để đạt tốc độ truyền tải tốt nhất về Việt Nam.
* **Branch**: `main`.
* **Root Directory**: `admin-backend` ⚠️ *(Rất quan trọng, để chỉ định deploy thư mục con `admin-backend/`)*.
* **Runtime**: `Node`.
* **Build Command**: `NODE_OPTIONS='--max-old-space-size=450' npm run build` 
  *(⚠️ Phải dùng tùy chọn `NODE_OPTIONS` này để giới hạn RAM khi build Next.js, tránh bị lỗi crash thiếu RAM ở gói Free của Render)*.
* **Start Command**: `npm run start`

### 3️⃣ Cấu Hình Biến Môi Trường (Environment Variables)
Cuộn xuống phần **Advanced**, click **Add Environment Variable** để thêm các giá trị dưới đây (lấy từ file `admin-backend/.env.local` của bạn):

| Key | Value | Mô tả / Lưu ý |
|-----|-------|---------------|
| `MONGODB_URI` | `mongodb+srv://tientantai12_db_user:xsTjCsDZYqfU84ne@myportfoliocluster.3ep6jh6.mongodb.net/` | URL MongoDB Atlas |
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
| `NEXT_PUBLIC_CLOUDINARY_CLOUD_NAME` | `dvfbzhr11` | Cloudinary Cloud name |
| `CORS_ORIGIN` | `https://<YOUR-FRONTEND-VERCEL-URL>.vercel.app` | **Điền URL trang Frontend trên Vercel của bạn** |

4. Chọn gói **Free** ở dưới cùng.
5. Click **Create Web Service**.

> ⏳ **Chờ Render build & deploy khoảng 2-3 phút.**
> Khi hoàn tất, Render sẽ cung cấp URL Backend ở góc trên bên trái, ví dụ:
> `https://admin-portfolio-backend.onrender.com`
>
> 📝 **Hãy copy URL này để cấu hình cho Vercel.**

---

## 🔗 BƯỚC 2: CẬP NHẬT BIẾN MÔI TRƯỜNG FRONTEND TRÊN VERCEL
Sau khi Backend trên Render đã chạy, bạn cần cập nhật thông tin kết nối cho Frontend trên Vercel.

1. Truy cập vào **[Vercel Dashboard](https://vercel.com/)** và chọn dự án Frontend `my-portfolio`.
2. Vào tab **Settings** → chọn **Environment Variables** (bên trái).
3. Thêm hoặc cập nhật 2 biến môi trường sau:
   * `VITE_API_URL` = `https://admin-portfolio-backend.onrender.com/api` (Nhớ thay bằng link Render backend của bạn + thêm `/api`)
   * `VITE_ADMIN_URL` = `https://admin-portfolio-backend.onrender.com/auth/login` (Nhớ thay bằng link Render backend của bạn + thêm `/auth/login`)
4. Click **Save**.

---

## 🔄 BƯỚC 3: REDEPLOY LẠI FRONTEND TRÊN VERCEL
Để dự án Frontend áp dụng các biến môi trường mới:

1. Trên Vercel, chuyển sang tab **Deployments**.
2. Click vào nút ba chấm `...` ở bản deploy mới nhất.
3. Chọn **Redeploy**.
4. Chờ Vercel build lại xong (~1 phút).

---

## 🔍 BƯỚC 4: KIỂM TRA
1. Truy cập vào trang web Frontend trên Vercel của bạn.
2. Nhấn **F12** → Chọn tab **Network**.
3. Reload lại trang và kiểm tra xem các request API tới Render backend đã nhận phản hồi thành công (`200 OK`) chưa.
4. Thử truy cập trang quản trị bằng link `VITE_ADMIN_URL` để kiểm tra chức năng đăng nhập.
