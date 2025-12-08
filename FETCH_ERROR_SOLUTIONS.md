# "Failed to Fetch" Error - Solutions

## 🔴 Error: Failed to fetch

This error means the frontend cannot connect to your backend API.

## ✅ Quick Fix: Use Mock Data

**Current Setting:** API_URL is now set to `""` (empty)

This means your app will use **mock data** for testing. You can:
1. Upload any file
2. Click "Analyze Sequences"
3. See results with sample data
4. Test the entire UI without a backend

**Perfect for:**
- Testing the UI
- Demos
- Development before backend is ready

## 🔧 To Connect to Real Backend

### Step 1: Check Backend Status

Make sure your backend is running:

```bash
# If running locally
python backend/main.py

# If running on Kaggle
# Check if your Kaggle notebook is running
```

### Step 2: Get Fresh ngrok URL

If using ngrok:

```bash
# Start ngrok
ngrok http 8000

# Copy the HTTPS URL (looks like: https://xxxx-xxxx-xxxx.ngrok-free.app)
```

**Important:** ngrok URLs expire! You need to:
- Restart ngrok each time
- Update the URL in your code

### Step 3: Update API_URL

In `src/components/UploadPage.tsx` line 6:

```typescript
// Replace with your NEW ngrok URL
const API_URL: string = "https://your-new-url.ngrok-free.app";
```

**No trailing slash!**

### Step 4: Verify Backend Endpoint

Your backend must have a POST endpoint at `/analyze`:

```python
@app.post("/analyze")
async def analyze(file: UploadFile):
    # Your processing code
    return {
        "status": "success",
        "data": {
            "metadata": {...},
            "sequences": [...],
            # etc.
        }
    }
```

### Step 5: Enable CORS

Your backend must allow requests from your frontend:

```python
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],  # For development
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

## 🧪 Test Backend Directly

Before testing from UI, verify backend works:

### Using curl:
```bash
curl -X POST "https://your-url.ngrok-free.app/analyze" \
  -F "file=@test.fasta" \
  -H "ngrok-skip-browser-warning: true"
```

### Using Browser:
1. Open browser console (F12)
2. Run:
```javascript
fetch('https://your-url.ngrok-free.app/analyze', {
  method: 'POST',
  headers: {
    'ngrok-skip-browser-warning': 'true'
  }
})
.then(r => r.json())
.then(d => console.log(d))
.catch(e => console.error(e));
```

## 🔍 Common Issues

### Issue 1: ngrok URL Expired
**Symptom:** "Failed to fetch"
**Solution:** Get new ngrok URL and update code

### Issue 2: Backend Not Running
**Symptom:** "Failed to fetch" or "Connection refused"
**Solution:** Start your backend server

### Issue 3: Wrong Endpoint
**Symptom:** 404 error
**Solution:** Ensure endpoint is `/analyze` not `/api/analyze`

### Issue 4: CORS Error
**Symptom:** "CORS policy" error in console
**Solution:** Add CORS middleware to backend

### Issue 5: Wrong HTTP Method
**Symptom:** 405 Method Not Allowed
**Solution:** Ensure backend accepts POST requests

### Issue 6: File Upload Issue
**Symptom:** 400 Bad Request
**Solution:** Check backend expects `file` field in FormData

## 📊 Current Setup

### Frontend (UploadPage.tsx)
```typescript
const API_URL: string = "";  // Empty = Mock data mode

// When ready for real backend:
const API_URL: string = "https://your-url.ngrok-free.app";
```

### What Happens:
- **Empty URL** → Uses mock data (3 second delay, sample results)
- **Valid URL** → Sends file to backend, gets real results

## 🎯 Recommended Workflow

### For Development/Testing:
1. **Use mock data** (API_URL = "")
2. Test all UI features
3. Verify everything works

### For Production/Demo:
1. **Start backend** on Kaggle/local
2. **Start ngrok** (if needed)
3. **Update API_URL** with fresh URL
4. **Test with real file**

## 💡 Pro Tips

### 1. Check Console Logs
Open browser console (F12) to see detailed logs:
```
🚀 Sending to Backend...
📁 File: your_file.fasta
🔗 API URL: https://...
📡 Response Status: 200
```

### 2. Use Mock Data First
Always test UI with mock data before connecting backend:
- Faster development
- No backend dependency
- Easier debugging

### 3. Keep ngrok Running
If using ngrok, keep the terminal window open:
- Closing terminal kills ngrok
- URL becomes invalid
- Need to restart and update code

### 4. Test Backend Separately
Before connecting frontend:
- Test backend with curl/Postman
- Verify it returns correct JSON
- Check CORS is enabled

### 5. Use Environment Variables
For production, use environment variables:
```typescript
const API_URL: string = process.env.NEXT_PUBLIC_API_URL || "";
```

## 🚀 Quick Start

### Right Now (Mock Data):
```typescript
const API_URL: string = "";  // ✅ Already set!
```

1. Refresh browser
2. Upload any file
3. Click "Analyze"
4. See mock results in 3 seconds

### When Backend Ready:
```typescript
const API_URL: string = "https://your-new-url.ngrok-free.app";
```

1. Start backend
2. Get ngrok URL
3. Update API_URL
4. Test with real file

## 📝 Checklist

Before connecting to backend:

- [ ] Backend is running
- [ ] ngrok is active (if using)
- [ ] Fresh ngrok URL copied
- [ ] API_URL updated in code
- [ ] CORS enabled on backend
- [ ] `/analyze` endpoint exists
- [ ] Endpoint accepts POST with file
- [ ] Returns JSON with correct format

## ✅ Current Status

**API_URL is now empty** - Your app will use mock data for testing!

You can:
- ✅ Upload files
- ✅ See loading animation
- ✅ View results page
- ✅ See report page
- ✅ Test all features

**No backend needed!**

When your backend is ready, just update the API_URL and you're good to go! 🎉
