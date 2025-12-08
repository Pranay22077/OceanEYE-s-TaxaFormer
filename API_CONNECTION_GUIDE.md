# API Connection Setup Guide

## ✅ What's Already Done

Your frontend is now fully connected to your backend! Here's what's implemented:

### UploadPage.tsx
- ✅ File upload with drag & drop
- ✅ Stores actual File objects (not just names)
- ✅ Sends files to backend via FormData
- ✅ Beautiful loading animation with 6 stages
- ✅ Progress bar and GPU activity indicators
- ✅ Saves results to localStorage
- ✅ Navigates to OutputPage on success
- ✅ Comprehensive error handling and logging

### OutputPage.tsx
- ✅ Reads results from localStorage on mount
- ✅ Displays real data from your backend
- ✅ Falls back to mock data if no results
- ✅ Shows metadata (sample name, sequences, time, confidence)
- ✅ Displays taxonomy summary, sequences table, and cluster data

## 🔧 Configuration

### Step 1: Update API URL

In `src/components/UploadPage.tsx`, line 6:

```typescript
const API_URL: string = "";  // Leave empty for mock data
// OR
const API_URL: string = "https://your-ngrok-url-here.ngrok-free.dev";
```

**Options:**
- **Leave empty (`""`)** - Uses mock data for testing the UI without a backend
- **Add your URL** - Connects to your actual backend API

Replace with your actual ngrok URL (no trailing slash) when ready.

### Step 2: Backend Response Format

Your backend must return JSON in this format:

```json
{
  "status": "success",
  "data": {
    "metadata": {
      "sampleName": "sample.fasta",
      "totalSequences": 150,
      "processingTime": "3.2s",
      "avgConfidence": 87
    },
    "taxonomy_summary": [
      { "name": "Alveolata", "value": 45, "color": "#22D3EE" },
      { "name": "Chlorophyta", "value": 32, "color": "#10B981" }
    ],
    "sequences": [
      {
        "accession": "SEQ_001",
        "taxonomy": "Alveolata; Dinoflagellata; Gymnodiniales",
        "length": 1842,
        "confidence": 0.94,
        "overlap": 87,
        "cluster": "C1"
      }
    ],
    "cluster_data": [
      { "x": 12.5, "y": 8.3, "z": 45, "cluster": "Alveolata", "color": "#22D3EE" }
    ]
  }
}
```

See `BACKEND_API_FORMAT.md` for complete details.

## 🧪 Testing

### Test with Mock Data (No Backend Required)

1. Keep `API_URL = ""` (empty)
2. Upload any file
3. Click "Analyze Sequences"
4. You'll see: `⚠️ No API URL configured - Using mock data`
5. After 3 seconds, you'll be redirected to results page with sample data

This is perfect for testing the UI before your backend is ready!

### Test 1: Check Console Logs

Open browser console (F12) and upload a file. You should see:

```
🚀 Sending to Backend...
📁 File: your_file.fasta
🔗 API URL: https://...
📡 Response Status: 200
📦 Received Result: {...}
✅ Analysis Complete!
📊 Data Structure: {...}
💾 Saved to localStorage
```

### Test 2: Verify localStorage

After successful upload, check localStorage in browser console:

```javascript
JSON.parse(localStorage.getItem('analysisResults'))
```

Should show your backend data.

### Test 3: Check OutputPage

Navigate to output page - it should display your real data, not mock data.

## 🐛 Troubleshooting

### Error: "Failed to fetch"
- Backend not running
- Wrong API URL
- CORS not enabled on backend

### Error: "HTTP 404"
- Wrong endpoint (should be `/analyze`)
- Backend route not configured

### Error: "HTTP 500"
- Backend crashed
- Check backend logs
- File format issue

### Data not showing on OutputPage
- Check browser console for errors
- Verify localStorage has data
- Check data format matches expected structure

## 🔍 Debugging Tips

### 1. Test Backend Directly

Use curl or Postman:

```bash
curl -X POST https://your-url.ngrok-free.dev/analyze \
  -F "file=@test.fasta" \
  -H "ngrok-skip-browser-warning: true"
```

### 2. Mock Data for Testing

To test UI without backend, run in browser console:

```javascript
localStorage.setItem('analysisResults', JSON.stringify({
  metadata: {
    sampleName: "test.fasta",
    totalSequences: 100,
    processingTime: "2.1s",
    avgConfidence: 92
  },
  taxonomy_summary: [
    { name: "Alveolata", value: 50, color: "#22D3EE" },
    { name: "Chlorophyta", value: 30, color: "#10B981" },
    { name: "Unknown", value: 20, color: "#64748B" }
  ],
  sequences: [
    {
      accession: "TEST_001",
      taxonomy: "Alveolata; Test; Species",
      length: 1500,
      confidence: 0.95,
      overlap: 90,
      cluster: "C1"
    },
    {
      accession: "TEST_002",
      taxonomy: "Chlorophyta; Test; Algae",
      length: 1200,
      confidence: 0.88,
      overlap: 85,
      cluster: "C2"
    }
  ],
  cluster_data: [
    { x: 10, y: 5, z: 50, cluster: "Alveolata", color: "#22D3EE" },
    { x: -5, y: 8, z: 30, cluster: "Chlorophyta", color: "#10B981" },
    { x: 2, y: -3, z: 20, cluster: "Unknown", color: "#64748B" }
  ]
}));
```

Then navigate to output page to see the data displayed.

### 3. Clear localStorage

If you need to reset:

```javascript
localStorage.removeItem('analysisResults');
```

## 📝 Backend Checklist

Make sure your backend has:

- [ ] POST endpoint at `/analyze`
- [ ] Accepts multipart/form-data with 'file' field
- [ ] CORS enabled (allow all origins for development)
- [ ] Returns JSON with `status` and `data` fields
- [ ] Handles errors gracefully
- [ ] Logs requests for debugging

## 🎯 Next Steps

1. Update API_URL in UploadPage.tsx
2. Ensure backend returns correct JSON format
3. Test with a real FASTA file
4. Check browser console for logs
5. Verify data appears on OutputPage

## 💡 Tips

- The loading animation runs for ~18 seconds (6 stages × 3 seconds)
- Progress bar updates every 200ms
- Real backend response will complete faster
- Frontend will jump to 100% when backend responds
- All console logs use emojis for easy scanning

## 🚀 Ready to Go!

Your frontend is fully configured and ready to connect to your backend. Just update the API_URL and ensure your backend returns the correct JSON format!
