# Student Answer Scripts – UTM

Automatic marking of scanned exam answer scripts. Each answer is classified as correct or incorrect, the marks (correct = 1, wrong = 0) are summed per student, and the result is compared with the lecturer's marks.

The work is split into 4 phases:

1. **CNN model selection**: compare three CNN models on answer classification (correct / incorrect)
2. **Automated marking (CNN)**: mark the students with the best CNN and compare with the lecturer's marks
3. **LSTM model development** *(to come)*: same classification, from the answer images converted into sequences (sequential image features)
4. **Automated marking (LSTM)** *(to come)*: same as phase 2, with the best LSTM

## Overview

 data/raw/                   original scanned scripts (SET 1 … SET 5)
     │
     │  01_cleaning.ipynb    red ink removal, cropping into exercises
     ▼
 data/cleaned/               1 image per exercise per student, unsorted
     │
     │  manual sorting       each exercise → correct/ or incorrect/
     ▼
 data/labeled/               labeled exercises
     │
     │  02_preprocessing_cnn.ipynb    grayscale, 224×224, normalization
     ▼
 data/processed/             .npy arrays ready for training
     │
     │  03_train_cnn.ipynb            phase 1: EfficientNetV2-S, V2-M, ConvNeXt-Tiny, 5-fold cross-validation
     ▼
 outputs/exercise_cnn/       per-model results + comparison table

The next notebooks will follow the order of the phases: phase 2 (CNN marking), then the LSTM part (preprocessing into sequences, training, marking).

## Folder structure

UTM/
├── data/                  ← not versioned (too large, can be regenerated)
│   ├── raw/
│   ├── cleaned/
│   ├── labeled/
│   └── processed/
├── notebooks/             ← run in numerical order
├── outputs/               ← training results (.pth not versioned, .png versioned)
└── docs/                  ← literature review, CNN/LSTM comparison tables, data-cleaning and preprocessing docs (EN/FR)
