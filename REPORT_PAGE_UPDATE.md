# Report Page - Real Data Integration

## ✅ Updated ReportPage to Use Real API Data

The ReportPage now dynamically loads and displays data from your backend API results!

### 🔄 What Changed

#### Before
- Static mock data hardcoded in the component
- Fixed values that never changed
- No connection to actual analysis results

#### After
- **Loads real data from localStorage** (same source as OutputPage)
- **Dynamically calculates all statistics** from actual sequences
- **Auto-generates charts and tables** from backend response
- **Handles missing data gracefully** with fallbacks

### 📊 Dynamic Features

#### 1. Summary Cards (Top Stats)
All calculated from real data:
- **Total Sequences** - Actual count from backend
- **Taxa Identified** - Unique taxonomy groups found
- **Confidence Average** - Mean confidence across all sequences
- **Novel/Unknown** - Sequences with low confidence or "Unknown" taxonomy

#### 2. Taxonomic Distribution Chart
- Automatically aggregates sequences by taxonomy
- Shows top 6 taxonomic groups
- Calculates percentages dynamically
- Color-coded bars

#### 3. Confidence Distribution Chart
- Shows average confidence per taxonomic group
- Bar heights represent confidence levels
- Uses same colors as taxonomy chart

#### 4. Top Taxa Table
- Lists top 5 most abundant taxa
- Shows sequence counts
- Displays average confidence per taxon
- Color-coded confidence badges:
  - 🟢 Green: ≥90% confidence
  - 🔵 Blue: 70-89% confidence
  - 🟡 Yellow: <70% confidence

#### 5. Metadata Section
Displays analysis information:
- Sample name
- Processing time
- Total sequences
- Average confidence

### 🎯 Data Flow

```
Backend API Response
        ↓
localStorage ('analysisResults')
        ↓
ReportPage loads on mount
        ↓
useMemo calculates stats
        ↓
Dynamic UI rendering
```

### 📋 What Gets Calculated

```typescript
// From your backend sequences
stats = {
  total: number,              // Total sequences
  taxaCount: number,          // Unique taxa
  avgConfidence: string,      // Average confidence %
  novelCount: number,         // Unknown/novel sequences
  taxonomyData: [...],        // For distribution chart
  topTaxa: [...]             // For detailed table
}
```

### 🔍 How It Works

#### 1. Load Data
```typescript
useEffect(() => {
  const savedData = localStorage.getItem('analysisResults');
  if (savedData) {
    setAnalysisData(JSON.parse(savedData));
  }
}, []);
```

#### 2. Calculate Stats
```typescript
const stats = useMemo(() => {
  // Aggregate sequences by taxonomy
  // Calculate averages
  // Count novel species
  // Generate chart data
}, [analysisData]);
```

#### 3. Render Dynamically
All UI elements use `stats` object - updates automatically when data changes.

### 🛡️ Error Handling

#### No Data State
If no analysis results found:
- Shows friendly message
- Provides "Go to Upload" button
- Doesn't crash or show errors

#### Loading State
While loading:
- Shows "Generating Report..." message
- Prevents rendering incomplete data

#### Missing Fields
Handles missing data gracefully:
- Uses fallback values
- Checks for undefined before accessing
- Filters out invalid entries

### 🎨 Features Added

#### 1. Back Button
Navigate back to OutputPage to see detailed results

#### 2. Share Button
Uses Web Share API (if available) to share results

#### 3. Export Button
Downloads analysis data as JSON file with timestamp

#### 4. Dynamic Colors
Uses consistent color scheme across all charts:
```typescript
const COLORS = ['#22D3EE', '#10B981', '#A78BFA', '#F59E0B', '#EC4899', '#64748B'];
```

### 📊 Data Requirements

The ReportPage works with the same data structure as OutputPage:

```json
{
  "metadata": {
    "sampleName": "...",
    "totalSequences": 150,
    "processingTime": "3.2s",
    "avgConfidence": 87
  },
  "sequences": [
    {
      "accession": "SEQ_001",
      "taxonomy": "Alveolata; Dinoflagellata",
      "confidence": 0.94,
      "cluster": "C1"
    }
  ]
}
```

### 🧪 Testing

1. **Upload and analyze a file** in UploadPage
2. **View results** in OutputPage
3. **Navigate to Report** (if you have navigation)
4. **See dynamic report** with your real data

Or manually test:
```javascript
// In browser console
localStorage.setItem('analysisResults', JSON.stringify({
  metadata: {
    sampleName: "test.fasta",
    totalSequences: 100,
    processingTime: "2.5s",
    avgConfidence: 92
  },
  sequences: [
    {
      accession: "SEQ_001",
      taxonomy: "Alveolata; Dinoflagellata",
      confidence: 0.95,
      cluster: "C1"
    },
    {
      accession: "SEQ_002",
      taxonomy: "Chlorophyta; Chlorophyceae",
      confidence: 0.88,
      cluster: "C2"
    }
  ]
}));
```

Then navigate to ReportPage to see it rendered!

### 💡 Key Improvements

✅ **Real-time data** - Shows actual analysis results
✅ **Dynamic calculations** - All stats computed from data
✅ **Flexible** - Works with any backend JSON format
✅ **Robust** - Handles missing/invalid data
✅ **Interactive** - Export and share functionality
✅ **Consistent** - Uses same data source as OutputPage

### 🎯 What You Get

- **Automatic aggregation** of taxonomy data
- **Confidence scoring** per taxonomic group
- **Novel species detection** (low confidence or "Unknown")
- **Visual charts** generated from real data
- **Detailed tables** with actual sequence counts
- **Export capability** for further analysis

### 🚀 Ready to Use!

Your ReportPage now displays real analysis results from your backend. Just upload a file, analyze it, and navigate to the report to see your data beautifully visualized!
