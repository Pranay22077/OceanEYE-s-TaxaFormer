# Supported JSON Formats

The OutputPage now automatically handles multiple JSON response formats from your backend!

## 🎯 Automatic Data Mapping

The frontend will automatically detect and map your backend JSON to the UI, regardless of the structure.

## ✅ Supported Formats

### Format 1: Structured (Recommended)
```json
{
  "metadata": {
    "sampleName": "sample.fasta",
    "totalSequences": 150,
    "processingTime": "3.2s",
    "avgConfidence": 87
  },
  "taxonomy_summary": [
    { "name": "Alveolata", "value": 45, "color": "#22D3EE" }
  ],
  "sequences": [
    {
      "accession": "SEQ_001",
      "taxonomy": "Alveolata; Dinoflagellata",
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
```

### Format 2: Flat Structure
```json
{
  "sampleName": "sample.fasta",
  "total_sequences": 150,
  "processing_time": "3.2s",
  "avg_confidence": 87,
  "results": [
    {
      "id": "SEQ_001",
      "classification": "Alveolata; Dinoflagellata",
      "seq_length": 1842,
      "score": 0.94
    }
  ]
}
```

### Format 3: Array of Sequences
```json
[
  {
    "sequence_id": "SEQ_001",
    "taxon": "Alveolata; Dinoflagellata",
    "length": 1842,
    "confidence": 0.94
  },
  {
    "sequence_id": "SEQ_002",
    "taxon": "Chlorophyta; Chlorophyceae",
    "length": 1654,
    "confidence": 0.89
  }
]
```

### Format 4: Alternative Field Names
```json
{
  "filename": "sample.fasta",
  "sequence_count": 150,
  "time": "3.2s",
  "confidence": 87,
  "classifications": [
    {
      "accession": "SEQ_001",
      "taxonomy": "Alveolata",
      "length": 1842,
      "confidence": 0.94
    }
  ],
  "taxonomySummary": [
    { "name": "Alveolata", "value": 45, "color": "#22D3EE" }
  ]
}
```

## 🔄 Field Mapping

The frontend automatically maps these field names:

### Sequences
- `sequences` OR `results` OR `classifications` OR array at root level
- Each sequence can have:
  - ID: `accession`, `id`, `sequence_id`
  - Taxonomy: `taxonomy`, `classification`, `taxon`
  - Length: `length`, `seq_length`
  - Confidence: `confidence`, `score`
  - Overlap: `overlap`, `coverage`
  - Cluster: `cluster`, `cluster_id`

### Taxonomy Summary
- `taxonomy_summary` OR `taxonomySummary` OR `taxonomy`
- Auto-generated from sequences if not provided

### Cluster Data
- `cluster_data` OR `clusterData` OR `clusters`

### Metadata
- Nested: `metadata.sampleName` OR flat: `sampleName`
- Sample Name: `sampleName`, `sample_name`, `filename`
- Total Sequences: `totalSequences`, `total_sequences`, `sequence_count`
- Processing Time: `processingTime`, `processing_time`, `time`
- Avg Confidence: `avgConfidence`, `avg_confidence`, `confidence`

## 🎨 Auto-Generated Features

### Taxonomy Summary
If your backend doesn't provide `taxonomy_summary`, the frontend will:
1. Extract the first taxonomy level from each sequence
2. Count occurrences
3. Assign colors automatically
4. Create the pie chart data

### Default Values
Missing fields get sensible defaults:
- Missing IDs → Auto-generated as `SEQ_001`, `SEQ_002`, etc.
- Missing taxonomy → `"Unknown"`
- Missing confidence → `0`
- Missing cluster → `"C1"`

## 🧪 Testing Your JSON

### Step 1: Check Console
After upload, open browser console (F12) and look for:
```
📊 Raw Analysis Results: {...}
📋 Result Keys: [...]
🔄 Transforming backend data...
✅ Transformation complete: {...}
```

### Step 2: Verify Transformation
The console will show:
```javascript
{
  taxonomyCount: 6,
  sequenceCount: 150,
  clusterCount: 6
}
```

### Step 3: Check for Errors
If something goes wrong, you'll see:
```
❌ Failed to parse analysis results: [error details]
```

## 💡 Tips

1. **Any structure works** - The transformer is flexible
2. **Check console logs** - Detailed logging shows what's happening
3. **Taxonomy auto-generated** - Don't worry if you don't have summary data
4. **Field names flexible** - Use snake_case or camelCase
5. **Missing data OK** - Defaults are provided

## 🚀 Example Backend Response

Here's a minimal working example:

```json
{
  "filename": "my_sample.fasta",
  "results": [
    {
      "id": "SEQ_001",
      "classification": "Alveolata; Dinoflagellata; Gymnodiniales",
      "length": 1842,
      "score": 0.94
    },
    {
      "id": "SEQ_002",
      "classification": "Chlorophyta; Chlorophyceae; Chlamydomonadales",
      "length": 1654,
      "score": 0.89
    }
  ]
}
```

This will automatically:
- Extract metadata (filename, sequence count)
- Map sequences with proper field names
- Generate taxonomy summary from classifications
- Create default cluster data
- Display everything beautifully in the UI!

## 📝 What You Need to Do

**Nothing!** Just return your JSON in any reasonable format and the frontend will handle it.

The only requirement: Your backend must return valid JSON with sequence/classification data.
