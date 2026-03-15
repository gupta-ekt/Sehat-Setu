# Sehat Setu - Complete Setup Guide

## 📋 Prerequisites

Before you begin, ensure you have the following installed:
- **Node.js** (v14 or higher) - [Download here](https://nodejs.org/)
- **MongoDB** (local or Atlas account) - [Download here](https://www.mongodb.com/try/download/community)
- **Git** (optional) - [Download here](https://git-scm.com/)
- **VS Code** with Live Server extension (for frontend)

## 🚀 Step-by-Step Setup

### Step 1: Install Backend Dependencies

Open terminal in the project root and run:

```bash
cd backend
npm install
```

This will install all required packages:
- express
- mongoose
- bcryptjs
- jsonwebtoken
- cors
- dotenv
- and more...

### Step 2: Configure Environment Variables

1. In the `backend` folder, copy `.env.example` to `.env`:
   ```bash
   cp .env.example .env
   ```

2. Open `.env` and configure:

**For Local MongoDB:**
```env
PORT=3000
NODE_ENV=development
MONGO_URI=mongodb://localhost:27017/sehat-setu
JWT_SECRET=sehat_setu_secret_key_2024_change_in_production
JWT_EXPIRE=7d
JWT_REFRESH_SECRET=sehat_setu_refresh_secret_2024
JWT_REFRESH_EXPIRE=30d
FRONTEND_URL=http://127.0.0.1:5500
```

**For MongoDB Atlas (Cloud):**
```env
PORT=3000
NODE_ENV=development
MONGO_URI=mongodb+srv://username:password@cluster.mongodb.net/sehat-setu?retryWrites=true&w=majority
JWT_SECRET=sehat_setu_secret_key_2024_change_in_production
JWT_EXPIRE=7d
JWT_REFRESH_SECRET=sehat_setu_refresh_secret_2024
JWT_REFRESH_EXPIRE=30d
FRONTEND_URL=http://127.0.0.1:5500
```

### Step 3: Setup MongoDB

**Option A: Local MongoDB**
1. Install MongoDB Community Edition
2. Start MongoDB service:
   - Windows: MongoDB runs as a service automatically
   - Mac: `brew services start mongodb-community`
   - Linux: `sudo systemctl start mongod`

**Option B: MongoDB Atlas (Recommended)**
1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Create a free account
3. Create a new cluster (M0 Free tier)
4. Click "Connect" → "Connect your application"
5. Copy the connection string
6. Replace `<password>` with your database password
7. Paste into `MONGO_URI` in `.env`

### Step 4: Start the Backend Server

In the `backend` folder:

```bash
# Development mode (auto-restart on changes)
npm run dev

# OR Production mode
npm start
```

You should see:
```
✅ MongoDB Connected Successfully
🚀 Server running on port 3000
📡 API available at http://localhost:3000/api
```

### Step 5: Setup Frontend

1. Open VS Code in the project root
2. Install "Live Server" extension if not already installed
3. Right-click on `frontend/index.html`
4. Select "Open with Live Server"

The frontend will open at `http://127.0.0.1:5500/frontend/index.html`

### Step 6: Verify Connection

1. Open browser console (F12)
2. Go to `http://127.0.0.1:5500/frontend/login.html`
3. Try to register a new account
4. Check the Network tab to see API calls

## 🔧 Frontend Configuration

The frontend is already configured to connect to the backend. The configuration is in `frontend/js/config.js`:

```javascript
const CONFIG = {
  API_BASE_URL: 'http://localhost:3000/api',
  FRONTEND_URL: 'http://127.0.0.1:5500',
  // ... other settings
};
```

**If your ports are different, update these values!**

## 📝 Testing the Setup

### Test 1: Backend Health Check

Open browser and visit:
```
http://localhost:3000/api/health
```

You should see:
```json
{
  "status": "success",
  "message": "Sehat Setu API is running",
  "timestamp": "2024-03-10T..."
}
```

### Test 2: Register a Patient

1. Go to `http://127.0.0.1:5500/frontend/login.html`
2. Click "Patient" tab
3. Click "Sign up here"
4. Fill in the registration form:
   - Name: Test Patient
   - Email: patient@test.com
   - Password: test123
   - Phone: +91 9876543210
   - Date of Birth: 1990-01-01
   - Gender: Male
   - Blood Group: O+
5. Click "Create Account"
6. You should see "Registration successful!"

### Test 3: Login

1. Switch to login form
2. Enter:
   - Email: patient@test.com
   - Password: test123
3. Click "Sign In"
4. You should be redirected to patient dashboard

### Test 4: Register a Doctor

1. Go back to login page
2. Click "Doctor" tab
3. Register with:
   - Name: Dr. Test
   - Email: doctor@test.com
   - Password: test123
   - Phone: +91 9876543210
   - License Number: MED123456
   - Specialty: General Medicine
   - Experience: 5

## 🔍 Troubleshooting

### Problem: "Cannot connect to MongoDB"

**Solution:**
- Check if MongoDB is running
- Verify MONGO_URI in .env
- For Atlas: Check IP whitelist (add 0.0.0.0/0 for testing)

### Problem: "CORS Error"

**Solution:**
- Ensure backend is running
- Check FRONTEND_URL in backend/.env matches your frontend URL
- Clear browser cache

### Problem: "Port 3000 already in use"

**Solution:**
```bash
# Windows
netstat -ano | findstr :3000
taskkill /PID <PID> /F

# Mac/Linux
lsof -ti:3000 | xargs kill -9
```

Or change PORT in .env to 3001

### Problem: "Module not found"

**Solution:**
```bash
cd backend
rm -rf node_modules package-lock.json
npm install
```

### Problem: Frontend not connecting to backend

**Solution:**
1. Check browser console for errors
2. Verify backend is running on port 3000
3. Check `frontend/js/config.js` has correct API_BASE_URL
4. Disable browser extensions (especially ad blockers)

## 📱 Frontend Updates Required

The login.html already has the correct API calls. Here's what's happening:

### Login Function (already implemented)
```javascript
const response = await fetch('/api/auth/login', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify(loginData)
});
```

### Register Function (already implemented)
```javascript
const response = await fetch('/api/auth/register', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify(registerData)
});
```

### Token Storage (already implemented)
```javascript
localStorage.setItem('userToken', data.tokens.accessToken);
localStorage.setItem('refreshToken', data.tokens.refreshToken);
localStorage.setItem('userId', data.user.id);
localStorage.setItem('userType', data.user.userType);
```

## 🎯 Next Steps

1. **Create Dashboard Pages**: Implement patient-dashboard.js and doctor-dashboard.js
2. **Add Appointment Booking**: Create appointment booking UI
3. **Implement File Upload**: Add document upload functionality
4. **Add AI Chat**: Integrate AI health assistant
5. **Testing**: Test all features thoroughly

## 📚 API Documentation

Full API documentation is available in `backend/README.md`

Key endpoints:
- `POST /api/auth/register` - Register user
- `POST /api/auth/login` - Login user
- `GET /api/auth/profile` - Get profile (requires token)
- `GET /api/doctors` - Get all doctors
- `POST /api/appointments` - Create appointment
- `GET /api/appointments` - Get appointments

## 🔐 Security Notes

For production:
1. Change JWT_SECRET to a strong random string
2. Use HTTPS
3. Enable rate limiting
4. Add input validation
5. Implement proper error handling
6. Use environment-specific configs
7. Enable MongoDB authentication

## 📞 Support

If you encounter issues:
1. Check the console logs (both frontend and backend)
2. Verify all environment variables
3. Ensure all services are running
4. Check MongoDB connection
5. Review the troubleshooting section

## ✅ Checklist

- [ ] Node.js installed
- [ ] MongoDB installed/Atlas account created
- [ ] Backend dependencies installed (`npm install`)
- [ ] .env file configured
- [ ] MongoDB running/connected
- [ ] Backend server started (`npm run dev`)
- [ ] Frontend opened with Live Server
- [ ] Health check endpoint working
- [ ] Registration working
- [ ] Login working
- [ ] Token stored in localStorage

Once all items are checked, your Sehat Setu application is ready! 🎉
