# Student Answer Scripts – UTM

Automatic classification of scanned exam answer scripts, with two goals:

1. **Exercise level**: is the answer to an exercise correct or incorrect? → **CNN**
2. **Student level**: based on all of their exercises, is the student good or not? → **LSTM**

---

## Overview

```
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
     │  02a_preprocessing_exercise.ipynb    grayscale, 224×224, normalization
     ▼
 data/processed/             .npy arrays ready for training
     │
     │  03_train_exercise_cnn.ipynb         EfficientNetV2-S, 5-fold cross-validation
     ▼
 outputs/exercise_cnn/       trained models + curves + confusion matrix
```

## Folder structure

```
UTM/
├── data/                  ← not versioned (too large, can be regenerated)
│   ├── raw/
│   ├── cleaned/
│   ├── labeled/
│   └── processed/
├── notebooks/             ← run in numerical order
├── outputs/               ← training results (.pth not versioned, .png versioned)
└── docs/                  ← literature review, CNN/LSTM comparison tables, preprocessing docs (EN/FR)
```
