# 🌐 Personal Portfolio Platform

> A full-stack personal portfolio website built to showcase projects, skills, and experience. Features a dual-frontend architecture, bilingual support (EN/VI), smooth animations, and a content management system via a separate admin dashboard.

🔗 **Live Demo:** [tientantai-portfolio.vercel.app](https://tientantai-portfolio.vercel.app)
🔗 **Admin Dashboard:** [my-portfolio-backend-red-omega.vercel.app](https://my-portfolio-backend-red-omega.vercel.app)

---

## 📸 Screenshots

<img width="1898" height="882" alt="image" src="https://github.com/user-attachments/assets/7de36c03-e85f-49e0-afba-6e4d946592b2" />


---

## ✨ Features

- 🎨 **Modern UI** — Clean, responsive design with smooth page transitions
- 🌍 **Bilingual Support** — English / Vietnamese via `react-i18next`
- ✨ **Animations** — High-performance animations with Framer Motion
- 🖥️ **Dual Frontend** — Separate user-facing site (React/Vite) and admin dashboard (Next.js)
- 📝 **Content Management** — Admin panel to manage projects, skills, and profile content
- 📬 **Contact Form** — Functional contact form integration
- ☁️ **Image Management** — Cloudinary integration for media uploads
- 🤖 **AI-Assisted Development** — Built with Cursor and Claude to accelerate workflow

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| User Frontend | React.js + Vite |
| Admin Dashboard | Next.js |
| Styling | Tailwind CSS, CSS Modules |
| Animations | Framer Motion |
| Internationalization | react-i18next |
| Database | MongoDB (Mongoose) |
| Backend | Node.js + Express (via admin-backend) |
| Media Storage | Cloudinary |
| Deployment | Vercel |
| Language | TypeScript (82%), CSS (13%) |

---

## 📁 Project Structure

```
personal-portfolio-platform/
├── src/                    # React user-facing frontend
│   ├── components/         # Reusable UI components
│   ├── pages/              # Page-level components
│   ├── i18n/               # EN/VI translation files
│   ├── animations/         # Framer Motion configs
│   └── assets/
├── admin-backend/          # Next.js admin dashboard + API
│   ├── pages/              # Admin pages and API routes
│   ├── components/
│   └── lib/                # DB connection, utilities
├── public/
├── CLOUDINARY_SETUP.md
├── DEPLOY-VERCEL.md
└── package.json
```

---

## 🚀 Getting Started

### Prerequisites

- Node.js >= 18
- MongoDB Atlas account
- Cloudinary account (for image uploads)

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/TTTai12/personal-portfolio-platform.git
cd personal-portfolio-platform

# 2. Install frontend dependencies
npm install

# 3. Set up environment variables
cp .env.example .env
```

### Environment Variables

```env
VITE_API_URL=http://localhost:3001
```

For the admin backend (`admin-backend/.env`):

```env
MONGODB_URI=your_mongodb_connection_string
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret
NEXTAUTH_SECRET=your_nextauth_secret
```

### Running Locally

```bash
# Start the user-facing frontend
npm run dev
# Runs at http://localhost:5173

# In a separate terminal, start the admin backend
cd admin-backend
npm install
npm run dev
# Runs at http://localhost:3001
```

### Deployment (Vercel)

See [DEPLOY-VERCEL.md](./DEPLOY-VERCEL.md) for step-by-step Vercel deployment guide.

---

## 🌍 Internationalization

The site supports English and Vietnamese. Translation files are located in `src/i18n/`. To add a new language, create a new locale file and register it in the i18n config.

---

## 👤 Author

**Nguyễn Tất Thành**
- Portfolio: [tientantai-portfolio.vercel.app](https://tientantai-portfolio.vercel.app)
- GitHub: [github.com/TTTai12](https://github.com/TTTai12)

> *Built Oct 2025 – Mar 2026*
