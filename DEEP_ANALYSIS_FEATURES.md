# Deep Analysis Report Features

## Overview
Enhanced the ReportPage component to provide comprehensive deep analysis of FASTA file analysis results received from the API in UploadPage.

## Data Flow
1. **UploadPage.tsx** → Receives FASTA file, sends to API, gets JSON results
2. **Results stored** → `localStorage.setItem('analysisResults', JSON.stringify(result.data))`
3. **ReportPage.tsx** → Loads data from localStorage and performs deep analysis

## Deep Analysis Metrics Implemented

### 1. **Biodiversity Metrics**
- **Shannon Index**: Measures species diversity accounting for abundance and evenness
  - Range: 0-5+
  - Interpretation: Higher = more diverse
  - Formula: H' = -Σ(pi × ln(pi))
  
- **Simpson Index**: Probability that two randomly selected individuals are different species
  - Range: 0-1
  - Interpretation: Higher = more diverse
  - Formula: D = 1 - Σ(pi²)
  
- **Evenness**: How evenly species are distributed in the community
  - Range: 0-1
  - Interpretation: Higher = more balanced community
  - Formula: E = H' / ln(S)

### 2. **Confidence Distribution Analysis**
- **High Confidence** (≥80%): Reliable classifications
- **Medium Confidence** (50-80%): Moderately reliable
- **Low Confidence** (<50%): Requires review
- Visual bar chart showing percentage breakdown

### 3. **Quality Assessment**
Based on confidence scores and overlap percentages:
- **Excellent Quality**: confidence ≥ 85% AND overlap ≥ 80%
- **Good Quality**: confidence 65-85% AND overlap ≥ 60%
- **Needs Review**: confidence < 65% OR overlap < 60%

### 4. **Taxonomic Level Completeness**
Tracks how many sequences have classification at each level:
- Kingdom
- Phylum
- Class
- Order
- Family
- Genus
- Species

Shows completion percentage for each taxonomic rank.

### 5. **Sequence Statistics**
- **Average Length**: Mean sequence length in base pairs
- **Min/Max Length**: Range of sequence lengths
- **Total Bases**: Total genetic material analyzed (in Megabases)

### 6. **Novelty Detection**
- **Novel Clusters**: Count of new/unknown clusters (cluster IDs starting with 'N')
- **Potential New Species**: Sequences with moderate confidence (30-60%) and unknown taxonomy
- **Uncertain Classifications**: Total sequences with confidence < 50%

## Visual Components

### Summary Cards (4 cards)
1. Total Sequences
2. Taxa Identified
3. Average Confidence
4. Novel/Unknown count

### Deep Analysis Section
- Gradient background (cyan to blue)
- 3 diversity metric cards with color-coded interpretations
- 2-column layout for confidence and quality distributions
- Sequence statistics grid
- Novelty detection panel

### Taxonomic Completeness Chart
- 7-column bar chart showing classification completeness
- Visual height represents percentage
- Actual counts and percentages displayed

### Existing Charts (Enhanced)
- Taxonomic Distribution (horizontal bars)
- Confidence Distribution (vertical bars)
- Top Identified Taxa (table)
- Analysis Metadata

## Color Coding

### Diversity Metrics
- **Green**: High/Good values
- **Yellow**: Moderate values
- **Orange**: Low values

### Confidence Levels
- **Green** (#10B981): High confidence
- **Orange** (#F59E0B): Medium confidence
- **Red** (#EF4444): Low confidence

### Quality Assessment
- **Green** (#10B981): Excellent
- **Blue** (#3B82F6): Good
- **Orange** (#F59E0B): Needs Review

## Data Structure Expected

The analysis expects JSON data from the API with this structure:

```json
{
  "metadata": {
    "sampleName": "sample.fasta",
    "totalSequences": 150,
    "processingTime": "2.8s",
    "avgConfidence": 89
  },
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
  "taxonomy_summary": [...],
  "cluster_data": [...]
}
```

## Usage

1. **Upload FASTA file** in UploadPage
2. **API processes** and returns results
3. **Results saved** to localStorage automatically
4. **Navigate to Report** to see comprehensive deep analysis
5. **Export data** as JSON using the Export button

## Interpretation Guidelines

### Shannon Index
- **> 2.5**: High diversity (healthy ecosystem)
- **1.5 - 2.5**: Moderate diversity
- **< 1.5**: Low diversity (stressed ecosystem)

### Simpson Index
- **> 0.8**: Highly diverse
- **0.5 - 0.8**: Moderately diverse
- **< 0.5**: Low diversity

### Evenness
- **> 0.7**: Balanced community (no dominant species)
- **0.4 - 0.7**: Somewhat balanced
- **< 0.4**: Dominated by few species

## Future Enhancements

Potential additions:
- Rarefaction curves
- Beta diversity comparisons (if multiple samples)
- Phylogenetic tree visualization
- Export to CSV/PDF reports
- Interactive filtering of sequences
- Temporal analysis (if time-series data)
- Geographic mapping (if location data available)

## Technical Details

- **React Hooks Used**: `useState`, `useEffect`, `useMemo`
- **Performance**: Calculations memoized to prevent unnecessary re-renders
- **Storage**: localStorage for data persistence
- **Responsiveness**: Grid layouts adapt to mobile/tablet/desktop
- **Dark Mode**: Full support with conditional styling
