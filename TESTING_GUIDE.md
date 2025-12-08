# Testing the Deep Analysis Report Feature

## Quick Test Guide

### 1. Start the Application
```powershell
cd d:\Coding\Taxaformer-Final\taxaformer
npm run dev
```

### 2. Test with Mock Data (No Backend Required)

#### Step 1: Configure UploadPage
Open `src/components/UploadPage.tsx` and ensure:
```typescript
const API_URL: string = "";  // Empty string = use mock data
```

#### Step 2: Upload a File
1. Navigate to Upload page
2. Drag & drop or select any `.fasta` file
3. Click "Analyze with AI"
4. Wait for mock processing (~3-5 seconds)
5. Automatically navigates to Output page

#### Step 3: View Deep Analysis
1. From Output page, look for "View Full Report" button
2. Or navigate to Report page directly
3. Should see comprehensive deep analysis

### 3. Test with Real Backend

#### Step 1: Start Backend
Ensure your Flask/FastAPI backend is running on ngrok.

#### Step 2: Configure API URL
Open `src/components/UploadPage.tsx` and set:
```typescript
const API_URL: string = "https://your-ngrok-url.ngrok-free.dev";
```

#### Step 3: Upload Real FASTA
1. Upload a real `.fasta` file
2. Wait for API processing
3. View results on Output page
4. Navigate to Report page for deep analysis

---

## What to Check

### ✅ Report Page Checklist

#### Summary Cards (Top Section)
- [ ] Total Sequences displays correct count
- [ ] Taxa Identified shows unique taxa
- [ ] Average Confidence percentage
- [ ] Novel/Unknown count

#### Deep Analysis Section
- [ ] Shannon Index calculated (0-5 range)
- [ ] Simpson Index calculated (0-1 range)
- [ ] Evenness calculated (0-1 range)
- [ ] Color-coded interpretation labels
- [ ] Confidence distribution bars
- [ ] Quality assessment breakdown
- [ ] Sequence statistics (length, bases)
- [ ] Novelty detection counts

#### Taxonomic Completeness
- [ ] 7 bars for taxonomic levels
- [ ] Heights represent percentages
- [ ] Counts displayed below bars
- [ ] Responsive on mobile

#### Charts Section
- [ ] Taxonomic distribution chart
- [ ] Confidence distribution chart
- [ ] Both render correctly
- [ ] Colors match COLORS array

#### Top Taxa Table
- [ ] Shows top 5 taxa
- [ ] Taxon names displayed
- [ ] Sequence counts correct
- [ ] Confidence badges color-coded

#### Metadata Section
- [ ] Sample name
- [ ] Processing time
- [ ] Total sequences
- [ ] Average confidence

---

## Mock Data Overview

The mock data includes:
- **150 total sequences**
- **6 different taxa**:
  - Alveolata (45)
  - Chlorophyta (32)
  - Fungi (15)
  - Metazoa (28)
  - Rhodophyta (18)
  - Unknown (12)
- **Confidence range**: 0.42 - 0.96
- **1 novel cluster** (N1)

---

## Expected Metrics (Mock Data)

### Shannon Index
Should be around **1.65 - 1.75**
- Moderate diversity

### Simpson Index
Should be around **0.75 - 0.80**
- Moderately diverse

### Evenness
Should be around **0.90 - 0.95**
- Well-balanced community

### Confidence Distribution
- High (≥80%): ~80 sequences (53%)
- Medium (50-80%): ~60 sequences (40%)
- Low (<50%): ~10 sequences (7%)

---

## Testing Scenarios

### Scenario 1: No Data
1. Clear localStorage: `localStorage.clear()`
2. Navigate to Report page
3. Should show "No Analysis Data Found" message
4. "Go to Upload" button should work

### Scenario 2: Mock Data
1. Upload any file with API_URL = ""
2. Wait for processing
3. Navigate to Report
4. All metrics should calculate correctly

### Scenario 3: Real Backend
1. Set real API_URL
2. Upload real FASTA file
3. Check all metrics match real data
4. Export JSON should download

### Scenario 4: Dark Mode
1. Toggle dark mode
2. Check all colors adapt
3. Gradient backgrounds should work
4. Text contrast should be readable

### Scenario 5: Responsive Design
1. Resize browser window
2. Mobile view (~375px)
3. Tablet view (~768px)
4. Desktop view (~1920px)
5. All grids should reflow properly

---

## Console Debugging

### Check localStorage
```javascript
// In browser console
const data = localStorage.getItem('analysisResults');
console.log(JSON.parse(data));
```

### Check Calculated Stats
The component logs to console:
```
📊 Report Page - Loaded Data: {...}
```

---

## Common Issues & Solutions

### Issue 1: "No Analysis Data Found"
**Solution**: 
- Go to Upload page first
- Upload and analyze a file
- Data should save to localStorage

### Issue 2: Metrics show 0 or NaN
**Solution**:
- Check data structure matches expected format
- Ensure sequences array exists
- Check confidence values are numbers (0-1 range)

### Issue 3: Charts not rendering
**Solution**:
- Check browser console for errors
- Ensure taxonomyData has items
- Verify COLORS array is defined

### Issue 4: Taxonomic levels empty
**Solution**:
- Check taxonomy strings use semicolon separators
- Format: "Kingdom; Phylum; Class; Order; Family; Genus; Species"
- Ensure proper parsing in stats calculation

---

## Browser Compatibility

Tested on:
- ✅ Chrome 120+
- ✅ Firefox 121+
- ✅ Safari 17+
- ✅ Edge 120+

---

## Performance Metrics

Expected load times:
- Data load from localStorage: **< 50ms**
- Stats calculation (150 sequences): **< 100ms**
- Initial render: **< 200ms**
- Total time to interactive: **< 500ms**

---

## Export Functionality

### Test Export
1. Click "Export" button
2. Should download JSON file
3. Filename format: `taxaformer-report-{timestamp}.json`
4. File should contain complete analysisData

### Test Share (if supported)
1. Click "Share" button
2. If navigator.share supported, should open share dialog
3. Otherwise, silently fails (no error)

---

## Accessibility

Check:
- [ ] Keyboard navigation works
- [ ] Screen reader labels present
- [ ] Color contrast ratios > 4.5:1
- [ ] Focus indicators visible
- [ ] All interactive elements accessible

---

## Next Steps After Testing

1. ✅ Verify all calculations correct
2. ✅ Test with various data sizes
3. ✅ Check mobile responsiveness
4. ✅ Validate dark mode
5. ✅ Test export functionality
6. 🚀 Deploy to production

---

**Testing Completed:** _____________  
**Tested By:** _____________  
**Issues Found:** _____________  
**Status:** _____________
