# DeepFashion (DF1) — Normalized Dataset for FEL

## 1. Dataset Overview

DeepFashion (DF1) is a large-scale fashion image dataset designed for clothing recognition and retrieval research.  
It reflects real-world conditions such as pose variation, deformation, occlusion, and lighting.

In this pipeline, the original DF1 dataset has been restructured into a **normalized schema** for use in the  
**FEL (Feature Evidence Layer or Knowledge-Graph-based Learning Layer)**.

### Key Characteristics

- Over **800,000 images** with rich annotations (categories, attributes, landmarks, etc.)
- Composed of five benchmarks:
  - In-shop Retrieval
  - Consumer-to-shop Retrieval
  - Category & Attribute Prediction (CAPB)
  - Landmark Detection
- All data unified using a single join key: `image_uid`
- Multimodal information stored in separate tables:
  - Text
  - Spatial data (bounding boxes)
  - Structural data (landmarks)
  - Labels (category/attribute)
  - Evaluation pairs
- Provenance preserved (`src_file + row_index`) for reproducibility

➡️ This converts DF1’s heterogeneous formats into a standardized schema optimized for multimodal learning and graph-based analysis.

---

## 2. Folder and File Structure

### (1) Original DF1 Structure (Input)

#### Evaluation Splits
- `list_eval_partition.txt` → Train / validation / test or query / gallery partition

#### Bounding Box Files
- `list_bbox_inshop.txt`
- `list_bbox_consumer2shop.txt`
- `list_bbox.txt` → Clothing object location

#### Landmark Files
- `list_landmarks.txt` → Keypoint coordinates

#### Category / Attribute Files
- `list_category_*`
- `list_attr_*` → Taxonomy definitions and labels

#### Text Descriptions
- `list_description_inshop.json` → Natural-language descriptions

---

### (2) Normalized Output Structure

Each benchmark is converted into:

benchmarks/
  df1_inshop/normalized/
  df1_consumer2shop/normalized/
  df1_capb_coarse/normalized/
  df1_capb_fine/normalized/
  df1_landmark/normalized/

#### Core Data Tables

- `eval_partition.csv.gz`
- `bbox.csv.gz`
- `landmarks_raw.csv.gz`
- `category_labels.csv.gz`
- `attr_sparse.csv.gz`
- `retrieval_pairs.csv.gz`
- `descriptions.csv.gz`
- `image_sources.csv.gz`

#### Ontology / Term Tables

- `category_terms.csv.gz`
- `attr_terms.csv.gz`

#### Management & Validation

- `manifest.csv` / `manifest.jsonl`
- `qc_summary.json`

---

## 3. Role in FEL

### Node Construction

- Image → `image_uid`
- Category → `category_terms`
- Attribute → `attr_terms`
- Text → `descriptions`
- Optional Item → bbox/landmark derived

### Relationship Construction

Image ─has_bbox──────────▶ BBoxEvidence  
Image ─has_landmark──────▶ LandmarkEvidence  
Image ─has_category──────▶ CategoryTerm  
Image ─has_attribute────▶ AttributeTerm  
Image ─has_description──▶ Text  
Image(query) ─matches───▶ Image(gallery)

### Core Design Principles

**Single Join Key**  
All data linked via `image_uid`

**Evidence Separation**  
Ontology / Labels / Evidence split

**Provenance Preservation**  
`src_file + row_index` stored

---

## 4. Extracted Benchmarks

### In-shop Retrieval
- Images: 52,712
- Text descriptions: 8,081
- No bbox/landmark errors

### Consumer-to-Shop Retrieval
- Images: 239,557
- Retrieval pairs: 195,540
- Real shopping scenario matching

### CAPB

Coarse:
- Images: 289,222
- Categories: 50
- Attributes: 1,000

Fine:
- Images: 20,000
- Attributes: 26

### Landmark Benchmark
- Images: 123,016
- No landmark errors

### Excluded Data
Original image files not included (metadata‑focused FEL design).

---

## 5. Graph Structure Description

### Central Node

**Images** — anchor for all connections

### Data Ingestion
ImageSources → Images (canonicalization)

### Labels & Attributes

CategoryTerms → CategoryLabels → Images  
AttrTerms → AttrSparse → Images  
ColorTerms → ColorLabels → Images

### Evaluation Structure

- EvalPartition → dataset splits
- RetrievalPairs → retrieval evaluation
- Descriptions → text data

### Reproducibility

Manifest → artifact audit  
QCSummary → pipeline validation

---

## DF1 Normalized Graph (Interactive)

Click below to open the interactive graph in a new window:

<a href="file:///Users/kimyounghoe/Desktop/Graph/DF1-3.html" target="_blank" rel="noopener noreferrer">
Open DF1 Graph Interactive Editor
</a>

⚠️ This link works only on the local machine where the file exists.

---

## Final Summary

The normalized DF1 dataset:

- Integrates complex original structures into a unified schema
- Optimized for multimodal AI and knowledge graph learning
- Includes reproducibility and quality validation
- Supports multiple benchmarks in a single data model

➡️ Core input for FEL‑based clothing recognition, retrieval, and recommendation systems.
