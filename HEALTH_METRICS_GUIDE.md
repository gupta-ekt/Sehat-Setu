# Health Metrics Feature - Complete Guide

## ✅ What's Implemented

### Hybrid Storage System
Your health readings are now saved in **TWO places**:

1. **localStorage (Browser)** - Instant, works offline
2. **MongoDB Database** - Persistent, accessible everywhere

### Features

#### 1. Add Health Readings
- Click "Add Reading" button on dashboard
- Select metric type (Weight, Blood Pressure, Heart Rate, etc.)
- Enter value and optional notes
- Saves instantly to localStorage
- Syncs to MongoDB in background

#### 2. View Health Metrics
- Displayed as colorful cards on dashboard
- Shows latest reading for each metric type
- Updates in real-time

#### 3. User Authentication
- Shows actual logged-in user's name
- Fetches profile from database if not in localStorage
- Redirects to login if not authenticated

## 📊 Available Metric Types

1. **Weight** (kg)
2. **Blood Pressure** (e.g., 120/80)
3. **Heart Rate** (bpm)
4. **Temperature** (°F)
5. **Blood Sugar** (mg/dL)
6. **BMI**
7. **Oxygen Level** (%)
8. **Cholesterol** (mg/dL)

## 🔌 API Endpoints

### Add Health Metric
```
POST /api/health-metrics
Authorization: Bearer <token>

Body:
{
  "type": "weight",
  "value": "70",
  "unit": "kg",
  "label": "Weight",
  "color": "#4facfe",
  "notes": "Morning weight",
  "recordedAt": "2024-03-11T10:00:00Z"
}
```

### Get All Metrics
```
GET /api/health-metrics
Authorization: Bearer <token>
```

### Get Latest Readings
```
GET /api/health-metrics/latest
Authorization: Bearer <token>
```

### Get Specific Metric
```
GET /api/health-metrics/:id
Authorization: Bearer <token>
```

### Update Metric
```
PUT /api/health-metrics/:id
Authorization: Bearer <token>

Body:
{
  "value": "72",
  "notes": "Updated reading"
}
```

### Delete Metric
```
DELETE /api/health-metrics/:id
Authorization: Bearer <token>
```

### Get Statistics
```
GET /api/health-metrics/stats/summary?days=30&type=weight
Authorization: Bearer <token>
```

## 🔄 How It Works

### When You Add a Reading:

1. **Instant Save** → Saves to localStorage immediately
2. **UI Update** → Dashboard updates instantly
3. **Background Sync** → Sends to MongoDB database
4. **Confirmation** → Shows success notification

### When You Load Dashboard:

1. **Quick Load** → Loads from localStorage first (instant)
2. **Fetch Latest** → Gets latest data from MongoDB
3. **Sync** → Updates localStorage with database data
4. **Display** → Shows most recent readings

## 📱 Where to See Your Data

### In Browser
1. **Dashboard** - Health Metrics section shows colored cards
2. **DevTools** - F12 → Application → Local Storage → `healthMetrics`
3. **Console** - Type: `JSON.parse(localStorage.getItem('healthMetrics'))`

### In Database
1. **MongoDB Atlas** - Go to your cluster → Browse Collections → `healthmetrics`
2. **MongoDB Compass** - Connect and view `sehat-setu` database
3. **API** - Call `GET /api/health-metrics` endpoint

## 🧪 Testing

### Test 1: Add a Reading
1. Login to patient dashboard
2. Click "Add Reading"
3. Select "Weight"
4. Enter "75"
5. Click "Save Reading"
6. See it appear in Health Metrics cards

### Test 2: Verify Database Save
1. Add a reading
2. Open MongoDB Atlas
3. Go to Collections → `healthmetrics`
4. See your new reading

### Test 3: Sync Across Devices
1. Add reading on one browser
2. Open dashboard in another browser (same account)
3. See the reading appear (synced from database)

### Test 4: Offline Mode
1. Disconnect internet
2. Add a reading
3. See it save to localStorage
4. Reconnect internet
5. Refresh page
6. Reading syncs to database

## 🔐 Security

- All API endpoints require JWT authentication
- Users can only see their own health metrics
- Data is encrypted in transit (HTTPS in production)
- MongoDB credentials stored in .env file

## 📈 Future Enhancements

Possible additions:
- Charts and graphs for trends
- Export to PDF
- Share with doctors
- Set health goals
- Reminders for readings
- Integration with wearable devices

## 🐛 Troubleshooting

### Reading not saving to database?
- Check if backend is running (`npm run dev` in backend folder)
- Check browser console for errors
- Verify you're logged in (check localStorage for `userToken`)

### Name showing as "John Doe"?
- Logout and login again
- Check if `userName` is in localStorage
- Backend will fetch from database automatically

### Metrics not loading?
- Check if backend is running on port 3001
- Check `frontend/js/config.js` has correct API_BASE_URL
- Clear localStorage and login again

## 📞 Support

If you encounter issues:
1. Check browser console (F12)
2. Check backend terminal for errors
3. Verify MongoDB connection
4. Check API endpoints with Postman

## ✨ Summary

You now have a fully functional health metrics system that:
- ✅ Saves readings to localStorage (instant)
- ✅ Syncs to MongoDB (persistent)
- ✅ Shows actual user's name
- ✅ Works offline
- ✅ Accessible from anywhere
- ✅ Secure and private

Enjoy tracking your health! 🎉
