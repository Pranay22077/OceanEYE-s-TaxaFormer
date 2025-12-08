# JSON Mapping - Quick Reference

## ✅ Your Backend Can Return ANY of These Formats!

The frontend automatically detects and maps your JSON structure.

## 🎯 What Gets Mapped

### 1. Sequences/Results
**Your backend field** → **UI displays as**

```
sequences          → Taxonomy Table
results            → Taxonomy Table  
classifications    → Taxonomy Table
[array at root]    → Taxonomy Table
```

Each sequence needs (flexible field names):
- **ID**: `accession` / `id` / `sequence_id` → Shows in "Accession" column
- **Taxonomy**: `taxonomy` / `classification` / `taxon` → Shows in "Predicted Taxonomy"
- **Length**: `length` / `seq_length` → Shows in "Length" column
- **Confidence**: `confidence` / `score` → Shows in "Confidence" column
- **Overlap**: `overlap` / `coverage` → Shows in "Overlap %" column
- **Cluster**: `cluster` / `cluster_id` → Shows in "Cluster" column

### 2. Taxonomy Summary (for Pie Chart)
**Your backend field** → **UI displays as**

```
taxonomy_summary   → Pie Chart
taxonomySummary    → Pie Chart
taxonomy           → Pie Chart
[auto-generated]   → Pie Chart (if not provided)
```

Format: `[{ name: "Alveolata", value: 45, color: "#22D3EE" }]`

**Don't have this?** No problem! It's auto-generated from your sequences.

### 3. Cluster Data (for UMAP Plot)
**Your backend field** → **UI displays as**

```
cluster_data       → Scatter Plot
clusterData        → Scatter Plot
clusters           → Scatter Plot
```

Format: `[{ x: 12.5, y: 8.3, z: 45, cluster: "Alveolata", color: "#22D3EE" }]`

### 4. Metadata (for Stats Display)
**Your backend field** → **UI displays as**

```
metadata.sampleName     → "Sample" stat
sampleName              → "Sample" stat
sample_name             → "Sample" stat
filename                → "Sample" stat

metadata.totalSequences → "Sequences" stat
totalSequences          → "Sequences" stat
total_sequences         → "Sequences" stat
sequence_count          → "Sequences" stat

metadata.processingTime → "Time" stat
processingTime          → "Time" stat
processing_time         → "Time" stat
time                    → "Time" stat

metadata.avgConfidence  → "Confidence" stat
avgConfidence           → "Confidence" stat
avg_confidence          → "Confidence" stat
confidence              → "Confidence" stat
```

## 📊 Example: Minimal Backend Response

```json
{
  "filename": "sample.fasta",
  "results": [
    {
      "id": "SEQ_001",
      "classification": "Alveolata; Dinoflagellata",
      "length": 1842,
      "score": 0.94
    }
  ]
}
```

**This automatically becomes:**

- ✅ Sample name: "sample.fasta"
- ✅ Total sequences: 1 (counted)
- ✅ Taxonomy table with 1 row
- ✅ Pie chart (auto-generated from "Alveolata")
- ✅ All UI components populated

## 🔍 Debugging

Open browser console (F12) after upload to see:

```
📊 Raw Analysis Results: { your JSON }
📋 Result Keys: ["filename", "results"]
🔄 Transforming backend data...
✅ Transformation complete: {
  taxonomyCount: 1,
  sequenceCount: 1,
  clusterCount: 0
}
```

## 💡 Pro Tips

1. **Use any field names** - snake_case, camelCase, whatever!
2. **Missing fields OK** - Defaults are provided
3. **No taxonomy summary?** - Auto-generated from sequences
4. **No cluster data?** - UI still works, just no scatter plot
5. **Check console** - Detailed logs show exactly what's happening

## 🚀 Bottom Line

**Just return your JSON with sequence data, and the frontend handles the rest!**

No need to match exact field names or structure. The transformer is smart enough to figure it out.
