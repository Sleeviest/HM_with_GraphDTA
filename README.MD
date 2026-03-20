# Integrating Network Pharmacology and Graph Neural Network–Based Drug–Target Affinity Prediction

This repository contains the analysis pipeline and processed data for the study:

**"Integrating Network Pharmacology and Graph Neural Network–Based Drug–Target Affinity Prediction for Sipjeondaebo-tang: A Pilot Analysis Focused on Astragali Radix"**

---

## Overview

Traditional herbal medicines contain multiple bioactive compounds that exert pharmacological effects through complex multi-target interactions.  
This study proposes a computational pipeline that integrates:

- **Network Pharmacology (NP)** → candidate compound–target generation  
- **Graph Neural Network–based Drug–Target Affinity Prediction (GraphDTA)** → structural prioritization  

The pipeline was applied to **Astragali Radix (Huangqi)** as a proof-of-concept case.

---

## Pipeline Summary

1. **Compound Curation**
   - Source: ETCM, TM-MC databases
   - Filtering:
     - Molecular weight ≥ 200 Da
     - Heavy atoms ≥ 14
   - Output: 149 curated phytochemicals

2. **Target Prediction**
   - Database: STITCH v5.0
   - Threshold: combined score ≥ 700
   - Output:
     - 168 compound–target interactions
     - 161 unique protein targets

3. **Axis Annotation**
   - Erythropoiesis-related axes:
     - HIF/EPO signaling
     - Iron metabolism
     - Microenvironment
   - Output: axis-tagged network

4. **Candidate Selection**
   - Axis targets + top STITCH-ranked targets
   - Degree filtering:
     - Max 15 targets per compound
     - Top 8 compounds per target
   - Output: 36 ligand–protein pairs

5. **GraphDTA Prediction**
   - Model: GINConvNet
   - Dataset: Davis benchmark
   - Output: predicted affinity scores (relative ranking)

---

## Repository Structure


.
├── data/
│ └── processed/
│ ├── compound_curated_druggable.tsv
│ ├── ct_stitch_score700_gene.tsv
│ ├── ct_tagged_score700.tsv
│ ├── graphdta_candidates_score700.tsv
│ └── graphdta_predictions_davis_val.csv
│
├── scripts/
│ ├── stage1_compound_curation.py
│ ├── stage2_stitch_network.py
│ ├── stage3_axis_annotation.py
│ ├── stage4_candidate_selection.py
│ └── stage5_graphdta_inference.py
│
├── notebooks/
│ └── analysis.ipynb
│
├── requirements.txt
└── README.md


---

## Data Sources

The following public databases were used:

- ETCM: http://www.tcmip.cn/ETCM/
- TM-MC: https://tm-mc.kisti.re.kr/
- STITCH: https://string-db.org

⚠️ **Note:**  
Raw STITCH data files are not included due to redistribution restrictions.  
Please download:

- `9606.protein_chemical.links.v5.0.tsv`
- `9606.protein.info.v12.0.txt`
- `9606.protein.sequences.v12.0.fa.gz`

---

## GraphDTA Dependency

This study uses the GraphDTA framework:

https://github.com/thinng/GraphDTA

Please clone it separately:

```bash
git clone https://github.com/thinng/GraphDTA.git
Environment

OS: Windows 11

Python: 3.10

PyTorch: 2.x

PyTorch Geometric: 2.x

RDKit: 2023.09

pandas: 2.x

numpy: 1.x

Cytoscape: 3.10.3

Reproducibility

To reproduce the analysis:

Prepare STITCH and compound datasets

Run scripts in order:

python stage1_compound_curation.py
python stage2_stitch_network.py
python stage3_axis_annotation.py
python stage4_candidate_selection.py
python stage5_graphdta_inference.py

Results will be saved in data/processed/

Key Results

149 curated compounds

168 high-confidence interactions

36 prioritized ligand–protein pairs

Top predicted interaction:

PPARG – pterocarpan derivative (score ~7.53)

The results suggest that Astragali Radix may influence erythropoiesis primarily through:

Iron metabolism (HMOX1, GDF15)

Metabolic regulation (PPARG)

Citation

If you use this code or data, please cite:

Kim G, Gyak K, Kwon S.
Integrating Network Pharmacology and Graph Neural Network–Based Drug–Target Affinity Prediction for Sipjeondaebo-tang.
License

This repository is provided for academic and research purposes only.

External resources (e.g., STITCH, GraphDTA) are subject to their respective licenses.

Contact

Gyeongmuk Kim
College of Korean Medicine, Gachon University
Email: gmkim@gachon.ac.kr