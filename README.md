# 📝 Madonoro Note App

A lightweight, full-stack note-taking application that allows users to create, manage, and organize personal notes securely from any device. Built with React, Express.js, and PostgreSQL (Supabase).

---

## 🚀 Quick Links

- **[⚡ Quick Start (5 min)](./QUICKSTART.md)** - Get up and running instantly
- **[🛠️ Development Guide](./DEVELOPMENT_GUIDE.md)** - Complete local setup
- **[📋 Project Structure](./PROJECT_STRUCTURE.md)** - Architecture & design
- **[🚢 Deployment Guide](./DEPLOYMENT.md)** - Deploy to production
- **[💻 VS Code Setup](./VSCODE_SETUP.md)** - Configure your IDE

---

## 📌 Project Overview

Madonoro Note App is a modern web application designed to provide users with a fast and reliable way to manage personal notes online.

Users can:
- ✅ Register and create accounts with secure authentication
- ✅ Log in with JWT-based sessions
- ✅ Create, read, update, and delete notes (CRUD)
- ✅ Access notes from any device
- ✅ Automatic timestamping for all notes
- ✅ Secure, user-specific data isolation

The system ensures that each user's data is isolated and secure, allowing only authenticated access to personal notes.

---

## 🎯 Key Features

| Feature | Description |
|---------|-------------|
| **User Authentication** | JWT-based login/registration system |
| **Note Management** | Create, read, update, delete notes |
| **Data Security** | User-specific data isolation |
| **Timestamps** | Automatic creation/update tracking |
| **Responsive Design** | Works on desktop and mobile devices |
| **Real-time Sync** | Instant note updates |
| **Cloud Database** | PostgreSQL on Supabase |

---

## 🏗️ System Architecture

```
┌─────────────────────┐
│  React Frontend     │
│ (cPanel/Netlify)    │
└──────────┬──────────┘
           │
           │ REST API
           ↓
┌─────────────────────┐
│  Express.js Backend │
│   (Render Cloud)    │
└──────────┬──────────┘
           │
           │ SQL
           ↓
┌─────────────────────┐
│   PostgreSQL DB     │
│   (Supabase Cloud)  │
└─────────────────────┘
```

### Tech Stack

| Layer | Technology |
|-------|-----------|
| **Frontend** | React 18, Vite, Tailwind CSS |
| **Backend** | Express.js, Node.js, JWT |
| **Database** | PostgreSQL (Supabase) |
| **Authentication** | JWT + bcryptjs |
| **Hosting** | Render (Backend), cPanel (Frontend), Supabase (Database) |
| **Development** | VS Code, Git, npm |

---

## 🔄 Data Flow

```
User Browser
    ↓
    │ Click & Type
    ↓
React Frontend (localhost:5173)
    ↓
    │ HTTP Request (with JWT)
    ↓
Express API (localhost:3000 / Render Cloud)
    ↓
    │ SQL Query
    ↓
PostgreSQL Database (Supabase Cloud)
    ↓
    │ Response
    ↓
React Frontend (Update UI)
    ↓
User Sees Changes
```

---

## 🎬 Getting Started

### Prerequisites

- **Node.js** 18+ ([Download](https://nodejs.org))
- **npm** or **yarn** (comes with Node.js)
- **Git** ([Download](https://git-scm.com))
- **A code editor** (VS Code recommended)

### 5-Minute Quick Start

```bash
# 1. Clone repository
git clone https://github.com/Speak-nice/Madonoro-Note-App.git
cd Madonoro-Note-App

# 2. Setup Backend
cd backend
npm install
npm run dev
# Backend runs on http://localhost:3000

# 3. In new terminal - Setup Frontend
cd frontend
npm install
npm run dev
# Frontend runs on http://localhost:5173
```

Visit: **http://localhost:5173**

### Full Setup Guide

Follow the **[DEVELOPMENT_GUIDE.md](./DEVELOPMENT_GUIDE.md)** for:
- Detailed environment setup
- Database configuration
- API testing
- Common troubleshooting

---

## 📁 Project Structure

```
Madonoro-Note-App/
├── frontend/                    # React application
│   ├── src/
│   │   ├── components/         # Reusable UI components
│   │   ├── pages/              # Page components
│   │   ├── hooks/              # Custom React hooks
│   │   ├── services/           # API client
│   │   ├── context/            # State management
│   │   └── styles/             # CSS files
│   └── package.json
│
├── backend/                     # Express.js API
│   ├── src/
│   │   ├── controllers/        # Route handlers
│   │   ├── routes/             # API routes
│   │   ├── middleware/         # Express middleware
│   │   ├── models/             # Data models
│   │   └── config/             # Configuration
│   └── package.json
│
├── database/                    # Database files
│   └── schema.sql              # PostgreSQL schema
│
└── docs/                        # Documentation files
    ├── QUICKSTART.md           # 5-minute setup
    ├── DEVELOPMENT_GUIDE.md    # Full development setup
    ├── DEPLOYMENT.md           # Production deployment
    ├── PROJECT_STRUCTURE.md    # Architecture overview
    └── VSCODE_SETUP.md         # IDE configuration
```

**[See Full Structure →](./PROJECT_STRUCTURE.md)**

---

## 🔌 API Endpoints

All endpoints require JWT authentication (except /auth endpoints).

### Authentication

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/register` | Create new account |
| POST | `/api/auth/login` | User login |

### Notes (Protected Routes)

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/notes` | Get all user notes |
| GET | `/api/notes/:id` | Get single note |
| POST | `/api/notes` | Create new note |
| PUT | `/api/notes/:id` | Update note |
| DELETE | `/api/notes/:id` | Delete note |

**[Full API Documentation →](./DEPLOYMENT.md#-api-endpoints)**

---

## 🚀 Deployment

### Frontend Deployment (cPanel)

```bash
# Build optimized version
cd frontend
npm run build

# Upload 'dist' folder to cPanel public_html
# Configure .htaccess for SPA routing
```

**[cPanel Setup Guide →](./DEPLOYMENT.md#-frontend-deployment-cpanel)**

### Backend Deployment (Render)

1. Push code to GitHub
2. Connect repository to Render
3. Set environment variables
4. Deploy (auto-deploys on push)

**[Render Setup Guide →](./DEPLOYMENT.md#-backend-deployment-render)**

### Database Deployment (Supabase)

1. Create Supabase project
2. Run SQL schema
3. Get API credentials
4. Add to environment variables

**[Supabase Setup Guide →](./DEPLOYMENT.md#-supabase-database-setup)**

---

## 🛠️ Development

### Setup VS Code

Install recommended extensions and configure debugging:

**[VS Code Setup Guide →](./VSCODE_SETUP.md)**

### Development Workflow

```bash
# Terminal 1: Backend
cd backend && npm run dev

# Terminal 2: Frontend
cd frontend && npm run dev

# Terminal 3: Testing
# Use Thunder Client or REST Client extension
```

### Testing Notes

Use the REST Client extension:

```http
### Register
POST http://localhost:3000/api/auth/register
Content-Type: application/json

{
  "email": "test@example.com",
  "username": "testuser",
  "password": "Password123!"
}

### Login
POST http://localhost:3000/api/auth/login
Content-Type: application/json

{
  "email": "test@example.com",
  "password": "Password123!"
}
```

---

## 📊 Database Schema

### Users Table
```sql
CREATE TABLE users (
  id BIGSERIAL PRIMARY KEY,
  email VARCHAR(255) UNIQUE NOT NULL,
  username VARCHAR(255) UNIQUE NOT NULL,
  password VARCHAR(255) NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Notes Table
```sql
CREATE TABLE notes (
  id BIGSERIAL PRIMARY KEY,
  user_id BIGINT NOT NULL REFERENCES users(id),
  title VARCHAR(255) NOT NULL,
  content TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## 🔐 Security Features

- ✅ **JWT Authentication** - Secure token-based auth
- ✅ **Password Hashing** - bcryptjs with salt rounds
- ✅ **CORS Protection** - Restricted cross-origin requests
- ✅ **User Data Isolation** - Each user sees only their notes
- ✅ **HTTPS/SSL** - Encrypted connections in production
- ✅ **Environment Variables** - Secrets not in code
- ✅ **Row Level Security** - Database-level protection (Supabase RLS)

---

## 🚀 Future Enhancements

- 📱 Mobile app (React Native)
- 🖥️ Desktop app (Electron)
- 📎 File attachments
- 👥 Note sharing & collaboration
- 🔄 Offline sync capability
- 🎨 Theme customization
- 🔍 Full-text search
- 📌 Note categories/tags

---

## 📚 Documentation

| Document | Purpose | Audience |
|----------|---------|----------|
| **QUICKSTART.md** | 5-minute setup | Everyone |
| **DEVELOPMENT_GUIDE.md** | Local development | Developers |
| **DEPLOYMENT.md** | Production deployment | DevOps/Deployers |
| **PROJECT_STRUCTURE.md** | Architecture overview | Developers |
| **VSCODE_SETUP.md** | IDE configuration | Developers |

---

## 🆘 Troubleshooting

### Backend won't start
```bash
# Check port 3000 is available
# Verify SUPABASE_URL and keys in .env
# Run: npm install (again)
```

### Frontend won't load
```bash
# Clear browser cache (Ctrl+Shift+Del)
# Check port 5173 is available
# Verify VITE_API_URL in .env
```

### Database connection errors
```bash
# Check SUPABASE_URL format
# Verify database tables exist
# Test connection: npm run db:test
```

**[Full Troubleshooting Guide →](./DEVELOPMENT_GUIDE.md#troubleshooting)**

---

## 📞 Support & Resources

- **Project Issues**: [GitHub Issues](https://github.com/Speak-nice/Madonoro-Note-App/issues)
- **React Documentation**: https://react.dev
- **Express.js Guide**: https://expressjs.com
- **Supabase Docs**: https://supabase.com/docs
- **Render Documentation**: https://render.com/docs

---

## 👨‍💻 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📄 License

This project is open source and available under the MIT License. It is intended for learning and development purposes.

---

## 🎓 Learning Outcomes

By working on this project, you'll learn:

- ✅ Full-stack web development
- ✅ React hooks and components
- ✅ Express.js REST API design
- ✅ PostgreSQL database design
- ✅ JWT authentication & authorization
- ✅ Cloud deployment (Render, Supabase, cPanel)
- ✅ Git version control
- ✅ Production-ready code practices

---

## ⭐ Show Your Support

If this project helped you, please:
- ⭐ Star the repository
- 🔗 Share with others
- 💬 Leave feedback in Issues
- 🤝 Contribute improvements

---

## 👤 Author

**Speak-nice** - [GitHub Profile](https://github.com/Speak-nice)

---

**Last Updated**: 2026-05-17  
**Status**: ✅ Production Ready  
**Version**: 1.0.0

---

## 📋 Quick Checklist

- [ ] Read [QUICKSTART.md](./QUICKSTART.md)
- [ ] Setup VS Code with [VSCODE_SETUP.md](./VSCODE_SETUP.md)
- [ ] Follow [DEVELOPMENT_GUIDE.md](./DEVELOPMENT_GUIDE.md)
- [ ] Understand [PROJECT_STRUCTURE.md](./PROJECT_STRUCTURE.md)
- [ ] Study [DEPLOYMENT.md](./DEPLOYMENT.md)
- [ ] Test all API endpoints
- [ ] Deploy to production
- [ ] Monitor and maintain

---

**Ready to build something amazing? Start with the [Quick Start Guide](./QUICKSTART.md)! 🚀**
