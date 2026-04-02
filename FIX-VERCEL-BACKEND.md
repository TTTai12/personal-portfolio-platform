# 🔧 FIX Backend Không Nhận Dữ Liệu Trên Vercel

## ⚠️ Vấn đề

- Frontend deploy trên Vercel nhưng gửi request tới `/api` (proxy cũ)
- Backend API đang chạy trên **domain khác** (separate Vercel app)
- CORS không được cấu hình đúng

---

## ✅ GIẢI PHÁP (5 BƯỚC)

### **Bước 1: Cập nhật Backend .env.local (LOCAL DEV)**

Thêm `CORS_ORIGIN` vào file `admin-backend/.env.local`:

```env
CORS_ORIGIN=http://localhost:5173
```

👉 `5173` là cổng mặc định của Vite dev server

---

### **Bước 2: Test Local**

1. **Terminal 1 - Backend** (cwd: `admin-backend`):

```bash
npm run dev
# Backend chạy tại http://localhost:3000
```

2. **Terminal 2 - Frontend** (cwd: `my-portfolio`):

```bash
npm run dev
# Frontend chạy tại http://localhost:5173
# Sẽ gọi tới http://localhost:3000/api theo .env.local
```

3. **Kiểm tra**: Vào frontend, xem Network tab có gọi tới backend không? Có dữ liệu không?

---

### **Bước 3: Deploy Backend (Nếu chưa)**

1. Vào **Vercel** → Tạo project từ `/admin-backend`
2. **Settings → Environment Variables**, thêm:

```
⚠️ QUAN TRỌNG: Lưu URL backend sau khi deploy xong
Ví dụ: https://admin-portfolio-abc123.vercel.app
```

---

### **Bước 4: Deploy Frontend**

1. Vào **Vercel** → Tạo project từ root (my-portfolio)
2. **Settings → Environment Variables**, thêm:

```
VITE_API_URL=https://admin-portfolio-abc123.vercel.app/api
VITE_ADMIN_URL=https://admin-portfolio-abc123.vercel.app/auth/login
```

**⚠️ Thay `admin-portfolio-abc123` bằng URL thực của backend**

3. **Redeploy** (trigger deploy mới)

---

### **Bước 5: Backend - Set CORS (Vercel)**

Vào Backend project **Settings → Environment Variables**, thêm/cập nhật:

```
CORS_ORIGIN=https://portfolio-xyz.vercel.app
```

**Hoặc cho phép tất cả (test):**

```
CORS_ORIGIN=*
```

**Sau đó Redeploy backend**

---

## 🧪 Kiểm Tra

### **Trên Vercel (Production)**

1. Frontend URL: https://portfolio-xyz.vercel.app
2. Mở **DevTools → Network tab**
3. Xem có request tới `admin-portfolio-abc123.vercel.app/api/**` không?
4. **Response** có dữ liệu không?

### **Dấu hiệu OK**

- ✅ Network: Request status `200`
- ✅ Response: `{ success: true, data: [...] }`
- ✅ Trang frontend hiển thị dữ liệu từ backend

### **Dấu hiệu LỖI**

- ❌ `CORS error` → Cập nhật `CORS_ORIGIN` backend
- ❌ `Cannot GET /api/...` → Frontend URL sai trong `VITE_API_URL`
- ❌ `Cannot connect` → Backend chưa deploy hoặc URL sai

---

## 📋 Checklist Deploy

- [ ] Frontend `.env.local` có `VITE_API_URL` (local)
- [ ] Backend `.env.local` có `CORS_ORIGIN=http://localhost:5173` (local)
- [ ] Backend deploy lên Vercel ✅
- [ ] Lấy URL backend thực (vd: `https://admin-xxx.vercel.app`)
- [ ] Frontend deploy lên Vercel
- [ ] Frontend Vercel Settings có `VITE_API_URL=https://admin-xxx.vercel.app/api`
- [ ] Backend Vercel Settings có `CORS_ORIGIN=https://portfolio-xxx.vercel.app`
- [ ] **Redeploy cả 2 project**
- [ ] Test Network tab → OK ✅

---

## 🐛 Debugging

**Nếu vẫn lỗi, kiểm tra:**

1. **Console Frontend:**

```javascript
console.log(import.meta.env.VITE_API_URL);
```

2. **Backend logs (Vercel):**

```
Vercel Dashboard → Deployments → Function logs
```

3. **Network tab:**

- Request URL có đúng backend domain không?
- Response Headers có `Access-Control-Allow-Origin` không?
