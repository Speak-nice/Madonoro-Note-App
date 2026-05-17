# 🏗️ Project Structure & Architecture

Complete system architecture, data flow, and component organization for the Madonoro Note App.

---

## 📋 Table of Contents

1. [System Architecture](#system-architecture)
2. [Directory Structure](#directory-structure)
3. [Data Models](#data-models)
4. [API Endpoints](#api-endpoints)
5. [Authentication Flow](#authentication-flow)
6. [Data Flow](#data-flow)
7. [Component Hierarchy](#component-hierarchy)

---

## 🏛️ System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     User's Browser                           │
│  ┌──────────────────────────────────────────────────────┐  │
│  │         React Frontend (localhost:5173)              │  │
│  │  ┌────────────────────────────────────────────────┐  │  │
│  │  │  Pages: Login, Register, Dashboard            │  │  │
│  │  │  Components: NoteCard, NoteForm, Nav           │  │  │
│  │  │  Services: API calls (Axios/Fetch)            │  │  │
│  │  │  State: Context API / React Query             │  │  │
│  │  └────────────────────────────────────────────────┘  │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                          ↕ (HTTP/REST)
                    https://API-URL/api/*
┌─────────────────────────────────────────────────────────────┐
│                  Express.js Backend API                      │
│              (localhost:3000 / Render.com)                   │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  Routes:                                             │  │
│  │  - POST   /api/auth/register                         │  │
│  │  - POST   /api/auth/login                            │  │
│  │  - GET    /api/notes (with JWT)                      │  │
│  │  - POST   /api/notes (with JWT)                      │  │
│  │  - PUT    /api/notes/:id (with JWT)                 │  │
│  │  - DELETE /api/notes/:id (with JWT)                 │  │
│  │                                                       │  │
│  │  Middleware:                                         │  │
│  │  - JWT Authentication                               │  │
│  │  - CORS Headers                                      │  │
│  │  - Error Handling                                    │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                          ↕ (SQL)
               @supabase/supabase-js
┌─────────────────────────────────────────────────────────────┐
│              PostgreSQL Database                             │
│              (Supabase Cloud)                                │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  Tables:                                             │  │
│  │  - users (id, email, username, password)             │  │
│  │  - notes (id, user_id, title, content, timestamps)  │  │
│  │                                                       │  │
│  │  Indexes:                                            │  │
│  │  - users.email (unique)                              │  │
│  │  - notes.user_id (for queries)                       │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

---

## 📁 Directory Structure

### Complete Folder Layout

```
Madonoro-Note-App/
│
├── 📁 frontend/                              # React Application
│   ├── 📁 src/
│   │   ├── 📁 components/
│   │   │   ├── Navigation.jsx                # Navigation bar
│   │   │   ├── NoteCard.jsx                  # Note display component
│   │   │   ├── NoteForm.jsx                  # Create/Edit form
│   │   │   ├── NotesList.jsx                 # Notes list view
│   │   │   └── ProtectedRoute.jsx            # Route guard
│   │   │
│   │   ├── 📁 pages/
│   │   │   ├── Login.jsx                     # Login page
│   │   │   ├── Register.jsx                  # Registration page
│   │   │   ├── Dashboard.jsx                 # Main dashboard
│   │   │   ├── NoteDetail.jsx                # Single note view
│   │   │   └── NotFound.jsx                  # 404 page
│   │   │
│   │   ├── 📁 services/
│   │   │   └── api.js                        # API client & calls
│   │   │
│   │   ├── 📁 hooks/
│   │   │   ├── useAuth.js                    # Auth logic hook
│   │   │   ├── useNotes.js                   # Notes logic hook
│   │   │   └── useFetch.js                   # Fetch wrapper hook
│   │   │
│   │   ├── 📁 styles/
│   │   │   ├── globals.css                   # Global styles
│   │   │   ├── components.css                # Component styles
│   │   │   └── tailwind.css                  # Tailwind config
│   │   │
│   │   ├── 📁 context/
│   │   │   └── AuthContext.jsx               # Auth state management
│   │   │
│   │   ├── App.jsx                           # Root component
│   │   ├── App.css                           # App styles
│   │   ├── main.jsx                          # Entry point
│   │   └── index.css                         # Base styles
│   │
│   ├── 📁 public/
│   │   ├── favicon.ico
│   │   └── index.html                        # Base HTML
│   │
│   ├── package.json                          # Dependencies
│   ├── package-lock.json                     # Lock file
│   ├── vite.config.js                        # Vite config
│   ├── .env                                  # Environment variables
│   ├── .env.example                          # Example env
│   ├── .eslintrc.json                        # ESLint config
│   ├── .prettierrc.json                      # Prettier config
│   └── index.html
│
├── 📁 backend/                               # Express.js API
│   ├── 📁 src/
│   │   ├── 📁 controllers/
│   │   │   ├── authController.js             # Auth logic
│   │   │   └── notesController.js            # Notes logic
│   │   │
│   │   ├── 📁 models/
│   │   │   ├── User.js                       # User model
│   │   │   └── Note.js                       # Note model
│   │   │
│   │   ├── 📁 routes/
│   │   │   ├── auth.js                       # Auth routes
│   │   │   └── notes.js                      # Notes routes
│   │   │
│   │   ├── 📁 middleware/
│   │   │   ├── auth.js                       # JWT middleware
│   │   │   ├── errorHandler.js               # Error handler
│   │   │   └── cors.js                       # CORS config
│   │   │
│   │   ├── 📁 config/
│   │   │   ├── supabase.js                   # DB connection
│   │   │   └── env.js                        # Env config
│   │   │
│   │   ├── 📁 utils/
│   │   │   ├── logger.js                     # Logging utility
│   │   │   └── validators.js                 # Input validation
│   │   │
│   │   └── server.js                         # Main server file
│   │
│   ├── package.json                          # Dependencies
│   ├── package-lock.json                     # Lock file
│   ├── .env                                  # Environment variables
│   ├── .env.example                          # Example env
│   ├── .eslintrc.json                        # ESLint config
│   ├── .prettierrc.json                      # Prettier config
│   └── .gitignore
│
├── 📁 database/
│   └── schema.sql                            # Database schema
│
├── 📁 docs/
│   ├── API.md                                # API documentation
│   └── ARCHITECTURE.md                       # Architecture docs
│
├── 📁 .vscode/
│   ├── settings.json                         # VS Code settings
│   ├── extensions.json                       # Extensions list
│   ├── launch.json                           # Debug config
│   └── tasks.json                            # Tasks config
│
├── README.md                                 # Project overview
├── DEVELOPMENT_GUIDE.md                      # Dev setup
├── DEPLOYMENT.md                             # Deployment steps
├── PROJECT_STRUCTURE.md                      # This file
├── VSCODE_SETUP.md                           # IDE setup
├── .gitignore                                # Git ignore
├── .prettierrc.json                          # Prettier config
├── .editorconfig                             # Editor config
└── LICENSE
```

---

## 📊 Data Models

### User Model

```javascript
{
  id: 1,                                    // Primary Key (Auto)
  email: "user@example.com",                // Unique, Required
  username: "myusername",                   // Unique, Required
  password: "$2b$10$...",                   // Hashed (bcrypt)
  created_at: "2026-05-17T10:30:00Z",      // Auto timestamp
  updated_at: "2026-05-17T10:30:00Z"       // Auto timestamp
}
```

### Note Model

```javascript
{
  id: 1,                                    // Primary Key (Auto)
  user_id: 1,                               // Foreign Key to users
  title: "My First Note",                   // Required
  content: "Note content here...",          // Optional, can be long text
  created_at: "2026-05-17T10:30:00Z",      // Auto timestamp
  updated_at: "2026-05-17T11:45:00Z"       // Auto timestamp
}
```

---

## 🔌 API Endpoints

### Authentication Endpoints

#### 1. Register User
```
POST /api/auth/register
Content-Type: application/json

Request Body:
{
  "email": "user@example.com",
  "username": "username",
  "password": "SecurePass123!"
}

Response (201):
{
  "success": true,
  "userId": 1,
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}

Errors:
- 400: Email/username already exists
- 400: Invalid email format
- 400: Password too weak
```

#### 2. Login User
```
POST /api/auth/login
Content-Type: application/json

Request Body:
{
  "email": "user@example.com",
  "password": "SecurePass123!"
}

Response (200):
{
  "success": true,
  "userId": 1,
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}

Errors:
- 401: Invalid email/password
- 404: User not found
```

### Notes Endpoints

#### 3. Get All Notes (Protected)
```
GET /api/notes
Authorization: Bearer <TOKEN>

Response (200):
{
  "success": true,
  "data": [
    {
      "id": 1,
      "title": "First Note",
      "content": "Content...",
      "created_at": "2026-05-17T10:30:00Z"
    },
    ...
  ]
}

Errors:
- 401: Unauthorized (no token)
- 403: Token invalid/expired
```

#### 4. Get Single Note (Protected)
```
GET /api/notes/:id
Authorization: Bearer <TOKEN>

Response (200):
{
  "success": true,
  "data": {
    "id": 1,
    "title": "My Note",
    "content": "Full content...",
    "created_at": "2026-05-17T10:30:00Z",
    "updated_at": "2026-05-17T10:30:00Z"
  }
}

Errors:
- 401: Unauthorized
- 404: Note not found
```

#### 5. Create Note (Protected)
```
POST /api/notes
Authorization: Bearer <TOKEN>
Content-Type: application/json

Request Body:
{
  "title": "New Note",
  "content": "Note content goes here"
}

Response (201):
{
  "success": true,
  "data": {
    "id": 2,
    "user_id": 1,
    "title": "New Note",
    "content": "Note content goes here",
    "created_at": "2026-05-17T11:00:00Z"
  }
}

Errors:
- 400: Title required
- 401: Unauthorized
```

#### 6. Update Note (Protected)
```
PUT /api/notes/:id
Authorization: Bearer <TOKEN>
Content-Type: application/json

Request Body:
{
  "title": "Updated Title",
  "content": "Updated content"
}

Response (200):
{
  "success": true,
  "data": {
    "id": 1,
    "title": "Updated Title",
    "content": "Updated content",
    "updated_at": "2026-05-17T12:00:00Z"
  }
}

Errors:
- 400: No fields to update
- 401: Unauthorized
- 404: Note not found
- 403: Not note owner
```

#### 7. Delete Note (Protected)
```
DELETE /api/notes/:id
Authorization: Bearer <TOKEN>

Response (200):
{
  "success": true,
  "message": "Note deleted successfully"
}

Errors:
- 401: Unauthorized
- 404: Note not found
- 403: Not note owner
```

---

## 🔐 Authentication Flow

### Flow Diagram

```
┌──────────────────┐
│   User          │
│   (Browser)      │
└────────┬─────────┘
         │
         │ 1. Submit email/password
         ↓
┌──────────────────────────────┐
│   Register/Login Page        │
│   (React Component)          │
└────────┬─────────────────────┘
         │
         │ 2. POST /api/auth/register
         │    or /api/auth/login
         ↓
┌────────────────────────────────┐
│   Backend API                  │
│   (Express.js Server)          │
│   1. Validate input            │
│   2. Hash password (bcryptjs)  │
│   3. Query database            │
│   4. Create/verify user        │
│   5. Generate JWT token        │
└────────┬───────────────────────┘
         │
         │ 3. Return token
         ↓
┌────────────────────────────────┐
│   Frontend                     │
│   1. Store token in localStorage
│   2. Redirect to dashboard    │
└────────────────────────────────┘
         │
         │ 4. For protected routes:
         │    Send: Authorization: Bearer TOKEN
         ↓
┌──────────────────────┐
│   JWT Middleware     │
│   1. Extract token   │
│   2. Verify token    │
│   3. Decode payload  │
│   4. Attach user info│
└──────────────────────┘
         │
         │ 5. Allow/Deny access
         ↓
┌──────────────────────┐
│   Route Handler      │
│   (Controller)       │
└──────────────────────┘
```

---

## 🔄 Data Flow

### Create Note Flow

```
User Types Note & Clicks Save
         ↓
Frontend: NoteForm Component
  - Validate input
  - Extract title & content
         ↓
API Call: POST /api/notes
  - Add Authorization header with token
  - Send JSON payload
         ↓
Backend: notesController.createNote()
  - Authenticate user (JWT middleware)
  - Validate input
  - Insert to database
  - Return new note object
         ↓
Frontend: Update state
  - Add note to list
  - Clear form
  - Show success message
         ↓
User Sees New Note in List
```

### Update Note Flow

```
User Clicks Edit Note
         ↓
Frontend: NoteForm Component
  - Pre-fill form with existing data
  - Allow editing
         ↓
User Updates Content & Saves
         ↓
API Call: PUT /api/notes/:id
  - Add Authorization header
  - Send updated data
         ↓
Backend: notesController.updateNote()
  - Authenticate user
  - Verify note ownership
  - Update in database
  - Return updated note
         ↓
Frontend: Update state
  - Update note in list
  - Close form
  - Show success message
         ↓
User Sees Updated Note
```

### Delete Note Flow

```
User Clicks Delete Button
         ↓
Confirmation Dialog
  "Are you sure?"
         ↓
User Confirms
         ↓
API Call: DELETE /api/notes/:id
  - Add Authorization header
         ↓
Backend: notesController.deleteNote()
  - Authenticate user
  - Verify ownership
  - Delete from database
  - Return success
         ↓
Frontend: Update state
  - Remove note from list
  - Show success message
         ↓
User Sees Note Removed
```

---

## 🎨 Component Hierarchy

```
App.jsx (Root)
├── AuthContext (Global State)
│   ├── Login Page
│   │   ├── Form Input
│   │   │   ├── Email Field
│   │   │   ├── Password Field
│   │   │   └── Submit Button
│   │   └── Link to Register
│   │
│   ├── Register Page
│   │   ├── Form Input
│   │   │   ├── Email Field
│   │   │   ├── Username Field
│   │   │   ├── Password Field
│   │   │   └── Submit Button
│   │   └── Link to Login
│   │
│   └── Dashboard (Protected)
│       ├── Navigation
│       │   ├── Logo
│       │   ├── User Info
│       │   └── Logout Button
│       │
│       ├── NoteForm
│       │   ├── Title Input
│       │   ├── Content Textarea
│       │   ├── Save Button
│       │   └── Cancel Button
│       │
│       └── NotesList
│           ├── Search/Filter
│           └── NoteCard (x many)
│               ├── Title
│               ├── Preview
│               ├── Timestamp
│               ├── Edit Button
│               └── Delete Button
```

---

## 📝 File Dependencies

### Frontend Dependencies

```
pages/Dashboard.jsx
├── components/Navigation.jsx
├── components/NoteForm.jsx
├── components/NotesList.jsx
│   └── components/NoteCard.jsx
├── hooks/useNotes.js
│   └── services/api.js
├── hooks/useAuth.js
│   └── context/AuthContext.jsx
└── styles/globals.css

pages/Login.jsx
├── services/api.js
├── context/AuthContext.jsx
├── hooks/useAuth.js
└── styles/globals.css
```

### Backend Dependencies

```
server.js
├── config/supabase.js
├── routes/auth.js
│   └── controllers/authController.js
│       ├── models/User.js
│       └── utils/validators.js
├── routes/notes.js
│   ├── middleware/auth.js
│   └── controllers/notesController.js
│       ├── models/Note.js
│       └── models/User.js
└── middleware/errorHandler.js
```

---

## 🔗 Technology Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Frontend** | React 18 | UI Framework |
| | Vite | Build Tool |
| | Tailwind CSS | Styling |
| | Axios/Fetch | HTTP Client |
| | Context API | State Management |
| **Backend** | Express.js | Web Framework |
| | Node.js | Runtime |
| | JWT | Authentication |
| | bcryptjs | Password Hashing |
| **Database** | PostgreSQL | RDBMS |
| | Supabase | Cloud DB & API |
| **Dev Tools** | Git | Version Control |
| | npm | Package Manager |
| | ESLint | Code Linting |
| | Prettier | Code Formatting |
| | VS Code | Editor |

---

Last Updated: 2026-05-17
Ready for development! 🚀
