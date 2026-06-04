# CaHCL: piRNA-disease association prediction based on large-scale model-guided semantic alignment and causal debiasing

This repository contains the official PyTorch implementation for the paper **"CaHCL: piRNA-disease association prediction based on large-scale model-guided semantic alignment and causal debiasing"**.

## 💡 Overview
CaHCL is a cross-modal "Teacher-Student" dual-stream framework designed to accurately identify piRNA-disease associations (PDAs) in extremely sparse networks. It synergizes **Causal Inference (IPW)** and **Large Language Models (LLMs)** to overcome topological data leakage, popularity bias, and false-negative collision traps inherent in conventional Graph Neural Networks (GNNs).

## ⚙️ Requirements
The code has been tested under Python 3.9.19. To install the required dependencies, run:

```bash
pip install numpy==1.26.4
pip install pandas==2.2.2
pip install torch==1.13.1
pip install torch-geometric==2.6.1
pip install torch-scatter==2.1.1+pt113cu116
pip install torch-sparse==0.6.17+pt113cu116
```

## 🚀 Quick Start
### Step 1: LLM Semantic Feature Extraction (Teacher Stream)
First, leverage the frozen pre-trained language models to extract high-fidelity semantic anchors.

#### Extract disease representations using PubMedBERT:
```python 
LLM/BioBERT_disease.py
```

#### Extract piRNA sequence representations using DNABERT-2:
```python
LLM/DNABERT-2_piRNA.py
```

### Step 2: Association Network Construction
Align the biological features with the bipartite graph topology to generate the required .pt and .npy files.
```python
processed/process_association.py
```

### Step 3: Train and Evaluate CaHCL (Student Stream & Alignment)
You can train the model directly using the main script. The framework is equipped with a DATASET_ZOO configuration to adaptively switch physical settings based on the target dataset's sparsity.

**1. Update the dataset name:** Open main.py and set your desired dataset:
```python
# In main_final.py, set this variable to 'MNDR' or 'PIRDISEASE'
TARGET_DATASET = 'PIRDISEASE'
```

**2. Update the data path:** Open utils/data_loader.py and ensure the data_dir parameter in the load_precomputed_data function points to the corresponding dataset folder:
```python
# In utils/data_loader.py, update the directory path
def load_precomputed_data(data_dir='piRDisease V1.0/processed'):
```

**3. Execute the training script:**
```python
main.py
```
The script will perform 5-fold cross-validation and output the comprehensive evaluation metrics (AUC, AUPR, Acc, F1, Pre, Sen) upon completion.
