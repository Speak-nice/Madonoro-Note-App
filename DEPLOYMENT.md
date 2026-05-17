# 🚀 Deployment Guide - Madonoro Note App

Complete deployment instructions for:
- **Frontend**: cPanel hosting
- **Backend**: Render (Cloud hosting)
- **Database**: Supabase (PostgreSQL)

---

## 📋 Table of Contents

1. [Supabase Database Setup](#supabase-database-setup)
2. [Backend Deployment (Render)](#backend-deployment-render)
3. [Frontend Deployment (cPanel)](#frontend-deployment-cpanel)
4. [Production Environment](#production-environment)
5. [Monitoring & Maintenance](#monitoring--maintenance)
6. [Post-Deployment Checklist](#post-deployment-checklist)

---

## 🗄️ Supabase Database Setup

### Step 1: Create Supabase Account

1. Go to https://supabase.com
2. Click "Sign Up"
3. Choose email or GitHub login
4. Create new organization (e.g., "Madonoro")

### Step 2: Create Project

1. Click "New Project"
2. **Project name**: `madonoro-production`
3. **Database password**: Create strong password (save it!)
4. **Region**: Choose closest to your users (e.g., US East)
5. Click "Create new project" (wait 2-3 minutes)

### Step 3: Get API Credentials

1. Go to **Settings → API**
2. Copy and save:
   - **Project URL**: `https://xxxxx.supabase.co`
   - **anon (public) key**: For frontend
   - **service_role key**: For backend (keep secret!)

### Step 4: Create Database Schema

1. Go to **SQL Editor** in Supabase dashboard
2. Click "New query"
3. Paste the schema from `database/schema.sql`:

```sql
-- Create users table
CREATE TABLE IF NOT EXISTS users (
  id BIGSERIAL PRIMARY KEY,
  email VARCHAR(255) UNIQUE NOT NULL,
  username VARCHAR(255) UNIQUE NOT NULL,
  password VARCHAR(255) NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Create notes table
CREATE TABLE IF NOT EXISTS notes (
  id BIGSERIAL PRIMARY KEY,
  user_id BIGINT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  title VARCHAR(255) NOT NULL,
  content TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Create indexes
CREATE INDEX idx_notes_user_id ON notes(user_id);
CREATE INDEX idx_users_email ON users(email);

-- Enable Row Level Security
ALTER TABLE users ENABLE ROW LEVEL SECURITY;
ALTER TABLE notes ENABLE ROW LEVEL SECURITY;
```

4. Click "Run"
5. Verify tables appear in **Table Editor**

### Step 5: Setup Row Level Security (RLS)

1. Go to **Authentication → Policies**
2. For `notes` table, add policies:
   - Users can only see/edit their own notes
   - Set `auth.uid()` matching `user_id`

---

## 🔧 Backend Deployment (Render)

### Step 1: Prepare Backend Code

Update `backend/package.json`:
```json
{
  "name": "madonoro-backend",
  "version": "1.0.0",
  "type": "module",
  "engines": {
    "node": "18.x"
  },
  "scripts": {
    "start": "node src/server.js",
    "dev": "nodemon src/server.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "cors": "^2.8.5",
    "dotenv": "^16.0.3",
    "@supabase/supabase-js": "^2.38.0",
    "jsonwebtoken": "^9.0.0",
    "bcryptjs": "^2.4.3"
  }
}
```

Create `.gitignore`:
```
node_modules/
.env
.env.local
.DS_Store
dist/
*.log
```

### Step 2: Create Render Account

1. Go to https://render.com
2. Sign up with GitHub (recommended for easy deployment)
3. Connect GitHub account

### Step 3: Deploy Backend

1. Go to **Dashboard → New +**
2. Select **Web Service**
3. **Select Repository**: Choose `Madonoro-Note-App`
4. **Fill in details**:
   - Name: `madonoro-api`
   - Environment: `Node`
   - Build Command: `npm install`
   - Start Command: `node src/server.js`
   - Plan: Free or Starter

5. **Add Environment Variables** (Settings):
   ```
   NODE_ENV=production
   PORT=3000
   SUPABASE_URL=https://xxxxx.supabase.co
   SUPABASE_ANON_KEY=your_anon_key
   SUPABASE_SERVICE_KEY=your_service_key
   JWT_SECRET=your_long_random_secret
   JWT_EXPIRE=7d
   FRONTEND_URL=https://your-cpanel-domain.com
   ```

6. Click **Deploy**
7. Wait for deployment (5-10 minutes)
8. Get your Render URL: `https://madonoro-api.onrender.com`

### Step 4: Test Render Deployment

```bash
# Test health endpoint
curl https://madonoro-api.onrender.com/api/health

# Should return:
# {"status":"OK","timestamp":"2026-05-17T..."}
```

### Step 5: Enable Auto-Deploy

In Render dashboard:
1. Go to your service settings
2. **Auto-Deploy**: Toggle "On"
3. Branch: `main`

Now every push to main branch auto-deploys!

---

## 🌐 Frontend Deployment (cPanel)

### Step 1: Build Frontend for Production

```bash
cd frontend

# Build optimized version
npm run build

# Output folder: frontend/dist/
# Contains static files ready for hosting
```

### Step 2: Prepare Frontend

Update `frontend/.env` for production:
```env
VITE_API_URL=https://madonoro-api.onrender.com
VITE_API_TIMEOUT=10000
```

Rebuild:
```bash
npm run build
```

### Step 3: Connect cPanel

#### Option A: Using cPanel File Manager

1. **Login to cPanel**
2. Go to **File Manager**
3. Navigate to **public_html** folder
4. Upload files from `frontend/dist/` folder
5. Upload all files and folders

#### Option B: Using Git (Recommended)

1. **In cPanel, open Terminal** (or SSH)
2. Navigate to public_html:
   ```bash
   cd ~/public_html
   ```

3. Clone repository:
   ```bash
   git clone https://github.com/Speak-nice/Madonoro-Note-App.git .
   ```

4. Install and build:
   ```bash
   cd frontend
   npm install
   npm run build
   
   # Copy dist contents to public_html
   cp -r dist/* /home/username/public_html/
   ```

#### Option C: Using FTP

1. Connect via FTP client (FileZilla, WinSCP)
2. Upload `dist/` folder contents to `public_html/`

### Step 4: Configure .htaccess for SPA Routing

Create `.htaccess` in `public_html/`:

```apache
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteBase /
  RewriteRule ^index\.html$ - [L]
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule . /index.html [L]
</IfModule>
```

This ensures all routes redirect to `index.html` for React Router.

### Step 5: Setup Custom Domain

1. **In cPanel → Domains**
2. Add your domain
3. Configure DNS:
   - **A Record**: Points to your cPanel IP
   - **CNAME**: If using subdomain

4. **Wait 24-48 hours for DNS propagation**

### Step 6: Enable HTTPS/SSL

1. **In cPanel → SSL/TLS**
2. Click "Auto Configuration"
3. Select your domain
4. Click "Install"

---

## 🔐 Production Environment

### Environment Variables

**Backend (`backend/.env` on Render)**:
```env
NODE_ENV=production
PORT=3000
SUPABASE_URL=https://xxxxx.supabase.co
SUPABASE_ANON_KEY=xxxx
SUPABASE_SERVICE_KEY=xxxx (KEEP SECRET!)
JWT_SECRET=long_random_string_min_32_chars
JWT_EXPIRE=7d
FRONTEND_URL=https://your-domain.com
CORS_ORIGIN=https://your-domain.com
LOG_LEVEL=info
```

**Frontend (`frontend/.env` for production build)**:
```env
VITE_API_URL=https://madonoro-api.onrender.com
VITE_API_TIMEOUT=10000
VITE_ENV=production
```

### Security Checklist

- [ ] All sensitive keys in environment variables (NOT in code)
- [ ] HTTPS enabled on frontend and backend
- [ ] CORS restricted to production domain
- [ ] JWT Secret is strong (32+ characters)
- [ ] Database backups enabled (Supabase auto-backups daily)
- [ ] Rate limiting enabled on API endpoints
- [ ] Input validation on all endpoints
- [ ] Password hashing with bcryptjs
- [ ] Remove console.log from production code

---

## 📊 Monitoring & Maintenance

### Render Dashboard Monitoring

1. **Metrics**: CPU, Memory, Requests
   - Check **Dashboard → Services → madonoro-api**
   - Monitor usage to avoid quota issues

2. **Logs**: View application logs
   - Click **Logs** tab
   - Useful for debugging errors

3. **Alerts**: Setup error notifications
   - Settings → Alerts
   - Get notified on deployment failures

### Supabase Monitoring

1. **Database**: Check connections and performance
   - **Monitoring → Database**
   - View slow queries

2. **Backups**: Automatic daily backups
   - **Settings → Database backups**
   - Manual backups available

### cPanel Monitoring

1. **Server Resources**: CPU, memory, bandwidth
   - **cPanel → System Metrics**

2. **Error Logs**: Application errors
   - **cPanel → Error Log**

3. **Database**: MySQL backups
   - **cPanel → Backups**

---

## Post-Deployment Checklist

### Phase 1: Verify Deployment

- [ ] Frontend loads on your domain
- [ ] Backend API responds to health check
- [ ] Database tables created and accessible
- [ ] No CORS errors in browser console

### Phase 2: Test Core Features

- [ ] User registration works
- [ ] User login works
- [ ] Create note functionality
- [ ] Read notes functionality
- [ ] Update note functionality
- [ ] Delete note functionality

### Phase 3: Security Tests

- [ ] Unauthenticated requests rejected
- [ ] Users can only access their own notes
- [ ] JWT tokens expire properly
- [ ] HTTPS certificate valid

### Phase 4: Performance Tests

- [ ] Frontend loads in < 3 seconds
- [ ] API responses in < 500ms
- [ ] No console errors
- [ ] Mobile responsive design works

### Phase 5: Setup Monitoring

- [ ] Render alerts configured
- [ ] Error logging setup
- [ ] Regular backup schedule confirmed
- [ ] SSL certificate renewal automated

---

## 🆘 Troubleshooting Deployment

### Frontend Not Loading

**Issue**: 404 errors or blank page
**Solution**:
1. Verify .htaccess is in public_html/
2. Check build output in dist/ folder
3. Clear browser cache (Ctrl+Shift+Del)
4. Verify custom domain DNS settings

### Backend Not Connecting

**Issue**: API calls failing
**Solution**:
1. Verify Render deployment successful
2. Check environment variables on Render
3. Test with curl: `curl https://api-url/api/health`
4. Check Supabase connection string

### Database Errors

**Issue**: Tables not found or query errors
**Solution**:
1. Verify schema SQL executed successfully
2. Check table names match code
3. Verify Supabase project settings
4. Test connection with Supabase dashboard

### CORS Issues

**Issue**: "Access to XMLHttpRequest blocked by CORS policy"
**Solution**:
1. Update CORS in backend:
   ```javascript
   cors({ origin: 'https://your-domain.com' })
   ```
2. Redeploy backend
3. Clear frontend cache

### SSL Certificate Issues

**Issue**: "Certificate not valid" warnings
**Solution**:
1. In cPanel, renew SSL certificate
2. Wait 1-2 hours for propagation
3. Clear browser cache
4. Test with https://www.sslshopper.com

---

## 📈 Performance Optimization

### Frontend Optimization

1. **Enable Gzip compression** (cPanel):
   - Settings → Gzip compression: ON

2. **Minify CSS/JS**:
   - Vite automatically minifies on build

3. **Lazy load routes** in React:
   ```javascript
   const Dashboard = React.lazy(() => import('./Dashboard'));
   ```

### Backend Optimization

1. **Enable caching headers**:
   ```javascript
   app.use((req, res, next) => {
     res.set('Cache-Control', 'public, max-age=3600');
     next();
   });
   ```

2. **Database indexing**:
   - Already in schema.sql
   - Monitor query performance

3. **Connection pooling**:
   - Handled by Supabase automatically

---

## 🔄 Continuous Deployment

### Auto-Deploy on Push

**Render** (Already configured):
- Every push to `main` branch auto-deploys

**cPanel** (Manual or Git hook):

Option 1: Manual rebuild
```bash
cd ~/public_html/frontend
git pull origin main
npm install
npm run build
```

Option 2: GitHub Actions (Advanced)
- Setup GitHub Actions workflow
- Auto-push to cPanel on main branch update

---

## 🆒 Domain & DNS Setup

### DNS Configuration

For domain `your-domain.com`:

| Type | Name | Value |
|------|------|-------|
| A | @ | your-cpanel-ip |
| A | www | your-cpanel-ip |
| CNAME | api | madonoro-api.onrender.com |

### Update Frontend API URL

After domain setup:
1. Update `frontend/.env`:
   ```env
   VITE_API_URL=https://api.your-domain.com
   ```
2. Rebuild and redeploy

---

## 📞 Support & Resources

- **Supabase Docs**: https://supabase.com/docs
- **Render Docs**: https://render.com/docs
- **cPanel Docs**: https://documentation.cpanel.net
- **Express.js**: https://expressjs.com
- **React**: https://react.dev

---

Last Updated: 2026-05-17
All systems production-ready! 🎉
