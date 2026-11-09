# Student Answer Scripts – UTM

Automatic marking of scanned exam answer scripts. Each answer is classified as correct or incorrect.

Current work: **CNN model selection**, comparing three CNN models on answer classification (correct / incorrect).


## Overview

```
 data/raw/                   original scanned scripts (SET 1, SET 2, ...)
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
     │  03_train_cnn.ipynb            EfficientNetV2-S, V2-M, ConvNeXt-Tiny, 5-fold cross-validation
     ▼
 outputs/exercise_cnn/       per-model results + comparison table
```

## Setup

```
pip install -r requirements.txt
```

A GPU is recommended for `03_train_cnn.ipynb`. To use it, install the CUDA build of `torch` and `torchvision` 
by following the command given on [pytorch.org](https://pytorch.org/get-started/locally/).

## Raw data

Only the folder structure of `data/` is versioned (empty `.gitkeep` files), the images are not. Put the scans in `data/raw/`, with one folder per student and one image per page:

```
data/raw/
├── SET 1/
│   ├── 01.jpg             ← cover page
│   ├── 02.jpg
│   ├── ...
│   └── 15.jpg
├── SET 2/
└── ...
```

## Manual labeling

This is the only step that is not automated. After `01_cleaning.ipynb`, copy each image of `data/cleaned/by_exercise/qXX/` 
into `data/labeled/by_exercise/qXX/correct/` or `data/labeled/by_exercise/qXX/incorrect/`, depending on the teacher's correction. 
Keep the file names (`set_X_qXX.jpg`), they tell which student and which exercise each image comes from.

## Folder structure

```
UTM/
├── data/                  ← only the folder structure is versioned (images too large and sensitive)
│   ├── raw/
│   ├── cleaned/
│   ├── labeled/
│   └── processed/
├── notebooks/             ← run in numerical order
├── outputs/               ← training results (.pth and .npy not versioned, .png versioned)
├── docs/                  ← literature review, CNN/LSTM comparison tables, data-cleaning, preprocessing and CNN training docs (EN/FR)
└── requirements.txt
```
