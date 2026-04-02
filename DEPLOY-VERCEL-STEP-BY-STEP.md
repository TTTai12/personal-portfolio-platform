# 🚀 HƯỚNG DẪN DEPLOY LẠI TỪ ĐẦU TRÊN VERCEL

> **Điều kiện:** Code đã push lên Git, bạn đã xóa deployment cũ

---

## **BƯỚC 1: DEPLOY BACKEND (admin-backend)**

### 1️⃣ Vào Vercel
- Link: [https://vercel.com](https://vercel.com)
- Đăng nhập tài khoản Vercel của bạn

### 2️⃣ Tạo Project Mới
- Click **Add New...** → **Project**
- Chọn **Import Git Repository**
- Tìm repo: `TTTai12/personal-portfolio-platform`
- Click **Import**

### 3️⃣ Cấu Hình Project
- **Project Name:** `admin-portfolio` (tùy ý)
- **Framework Preset:** `Next.js` (tự động detect)
- **Root Directory:** Chọn `admin-backend` ✅
- Click **Deploy**

⏳ **Chờ deploy (2-3 phút)**... 

Khi xong sẽ thấy URL: `https://admin-portfolio-xxxxx.vercel.app`
📝 **Lưu URL này!**

---

## **BƯỚC 2: CẤU HÌNH ENVIRONMENT VARIABLES (BACKEND)**

### 1️⃣ Vào Settings
- Dashboard → Project: `admin-portfolio`
- Tab **Settings**
- Chọn **Environment Variables** (bên trái)

### 2️⃣ Thêm Tất Cả Variables

Nhập từng dòng (copy từ file `admin-backend/.env.local`):

| Key | Value |
|-----|-------|
| `MONGODB_URI` | `mongodb+srv://tientantai12_db_user:xsTjCsDZYqfU84ne@myportfoliocluster.3ep6jh6.mongodb.net/` |
| `NEXTAUTH_SECRET` | `+vSh4uq7MuUHgQLhJH6edd+t7kQA1Y+y2GujRN0bonU=` |
| `NEXTAUTH_URL` | `https://admin-portfolio-xxxxx.vercel.app` ⚠️ (thay xxxxx) |
| `ADMIN_USERNAME` | `admin` |
| `ADMIN_PASSWORD` | `admin123` |
| `EMAIL_USER` | `tientantai12@gmail.com` |
| `EMAIL_PASS` | `lwfwhqkqrazpyiib` |
| `EMAIL_TO` | `tientantai12@gmail.com` |
| `EMAIL_HOST` | `smtp.gmail.com` |
| `EMAIL_PORT` | `465` |
| `EMAIL_SECURE` | `true` |
| `NEXT_PUBLIC_CLOUDINARY_CLOUD_NAME` | `dvfbzhr11` |
| `CORS_ORIGIN` | `*` (tạm, sẽ cập nhật sau) |

**Cách thêm:**
- Click **"Add New"**
- **Name:** (key từ bảng trên)
- **Value:** (value từ bảng trên)
- Click **Save**
- Repeat cho tất cả

### 3️⃣ Redeploy
- Vào **Deployments**
- Click **...** trên deployment mới nhất
- Chọn **Redeploy** ✅

⏳ Chờ redeploy xong (~2 phút)

---

## **BƯỚC 3: DEPLOY FRONTEND (my-portfolio)**

### 1️⃣ Tạo Project Mới
- Dashboard Vercel → **Add New... → Project**
- **Import Git Repository** → `TTTai12/personal-portfolio-platform`
- Click **Import**

### 2️⃣ Cấu Hình Project
- **Project Name:** `portfolio` (tùy ý)
- **Framework Preset:** `Vite`
- **Root Directory:** `.` (root, mặc định) ✅
- **Build Command:** `npm run build` (mặc định)
- **Output Directory:** `dist` (mặc định)
- Click **Deploy**

⏳ Chờ deploy xong...

URL: `https://portfolio-xxxxx.vercel.app`
📝 **Lưu URL này!**

---

## **BƯỚC 4: CẤU HÌNH ENVIRONMENT VARIABLES (FRONTEND)**

### 1️⃣ Vào Settings
- Dashboard → Project: `portfolio`
- Tab **Settings**
- **Environment Variables**

### 2️⃣ Thêm 2 Variables

| Key | Value |
|-----|-------|
| `VITE_API_URL` | `https://admin-portfolio-xxxxx.vercel.app/api` ⚠️ |
| `VITE_ADMIN_URL` | `https://admin-portfolio-xxxxx.vercel.app/auth/login` ⚠️ |

⚠️ **Thay `admin-portfolio-xxxxx` bằng URL backend thực của bạn**

**Ví dụ cụ thể:**
```
VITE_API_URL = https://admin-portfolio-tau.vercel.app/api
VITE_ADMIN_URL = https://admin-portfolio-tau.vercel.app/auth/login
```

### 3️⃣ Redeploy
- **Deployments** → Click **...** → **Redeploy**

⏳ Chờ redeploy xong

---

## **BƯỚC 5: CẬP NHẬT CORS (BACKEND)**

Giờ backend biết frontend URL, hãy cập nhật CORS:

### 1️⃣ Vào Backend Settings
- Dashboard → Project: `admin-portfolio`
- **Settings → Environment Variables**

### 2️⃣ Sửa CORS_ORIGIN
- Tìm biến `CORS_ORIGIN`
- Sửa giá trị từ `*` thành: `https://portfolio-xxxxx.vercel.app` ⚠️
- Click **Save**

**Ví dụ:**
```
CORS_ORIGIN = https://portfolio-tau.vercel.app
```

### 3️⃣ Redeploy Backend
- **Deployments** → Click **...** → **Redeploy**

⏳ Chờ xong (~2 phút)

---

## **BƯỚC 6: KIỂM TRA**

### 1️⃣ Truy Cập Frontend
- Vào: `https://portfolio-xxxxx.vercel.app` (URL frontend)
- Trang phải load bình thường

### 2️⃣ Mở DevTools
- Nhấn **F12** hoặc **Chuột phải → Inspect**
- Vào tab **Network**

### 3️⃣ Reload Trang
- Nhấn **Ctrl+R** (hoặc F5)

### 4️⃣ Tìm Request API
- Trong Network tab, tìm request tới: `/api/products`, `/api/about`, `/api/projects`, etc.
- Xem **Status** có phải `200` không?

### 5️⃣ Kiểm Tra Response
- Click vào request → Tab **Response**
- Xem có dữ liệu không?

---

## **✅ KẾT QUẢ MONG MUỐN**

### Nếu OK:
- ✅ Request Status: `200`
- ✅ Response: `{ success: true, data: [...] }`
- ✅ Trang Frontend hiển thị dữ liệu
- ✅ Admin page (`/auth/login`) load được

### Nếu Lỗi:

**❌ CORS Error (blocked by CORS policy):**
```
Access to XMLHttpRequest at 'https://admin-xxx.vercel.app/api/...' 
from origin 'https://portfolio-xxx.vercel.app' has been blocked
```
→ Kiểm tra `CORS_ORIGIN` ở backend, phải match frontend URL

**❌ 404 Not Found:**
```
GET https://admin-xxx.vercel.app/api/projects 404
```
→ Kiểm tra `VITE_API_URL` ở frontend, phải đúng URL backend

**❌ Cannot Connect / Timeout:**
→ Backend chưa deploy xong, hoặc có lỗi
→ Vào Backend → Deployments → Xem **Function logs**

---

## **📋 CHECKLIST DEPLOY**

```
BACKEND:
- [ ] Deploy admin-backend lên Vercel
- [ ] Lấy URL backend: https://admin-portfolio-xxxxx.vercel.app
- [ ] Thêm tất cả 13 environment variables
- [ ] Redeploy backend
- [ ] Test: Truy cập https://admin-portfolio-xxxxx.vercel.app/api/projects
      → Có dữ liệu JSON không?

FRONTEND:
- [ ] Deploy my-portfolio lên Vercel
- [ ] Lấy URL frontend: https://portfolio-xxxxx.vercel.app
- [ ] Thêm VITE_API_URL = https://admin-portfolio-xxxxx.vercel.app/api
- [ ] Thêm VITE_ADMIN_URL = https://admin-portfolio-xxxxx.vercel.app/auth/login
- [ ] Redeploy frontend

FINAL:
- [ ] Backend: Sửa CORS_ORIGIN = https://portfolio-xxxxx.vercel.app
- [ ] Backend: Redeploy
- [ ] Test Network tab → Status 200
- [ ] Frontend: Hiển thị dữ liệu từ backend → ✅
- [ ] Admin trang: Load được → ✅
```

---

## **❓ CÂU HỎI THƯỜNG GẶP**

**Q: Làm sao biết URL backend?**
A: Khi deploy xong → Vercel Dashboard → Project → Trang đầu tiên hiển thị URL

**Q: Sao không thấy dữ liệu?**
A: 
1. Mở DevTools → Network → Xem Request có đi tới backend không?
2. Xem Response có dữ liệu không?
3. Nếu CORS error → Update CORS_ORIGIN
4. Nếu 404 → Update VITE_API_URL

**Q: Cần redeploy bao nhiêu lần?**
A: 
- Backend: 2 lần (sau khi deploy, sau khi add env vars, sau khi update CORS)
- Frontend: 2 lần (sau khi deploy, sau khi add env vars)

**Q: Có cách nào nhanh hơn không?**
A: Có, bạn có thể add tất cả env vars **trước** deploy, nhưng thường add sau dễ quên.

---

**Bạn thực hiện từng bước, gặp lỗi nào thì báo cho tôi! 👍**
