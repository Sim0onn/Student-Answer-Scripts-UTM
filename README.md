# Student Answer Scripts – UTM

Automatic marking of scanned exam answer scripts. Each answer is classified as correct or incorrect.

Current work: **CNN model selection**, comparing three CNN models on answer classification (correct / incorrect).


## Overview

```
 data/raw/                                 original scanned scripts (SET 1, SET 2, ...)
     │
     │  01_cleaning.ipynb                  red ink removal, cropping into exercises
     ▼
 data/cleaned/                             1 image per exercise per student, unsorted
     │
     │  manual sorting                     each exercise → correct/ or incorrect/
     ▼
 data/labeled/                             labeled exercises
     │
     │  classifier/02_preprocessing.ipynb  grayscale, 224×224, normalization
     ▼
 data/processed/                           .npy arrays ready for training
     │
     │  classifier/03_train.ipynb          EfficientNetV2-S, V2-M, ConvNeXt-Tiny, 5-fold cross-validation
     ▼
 outputs/predictions/classifier.csv       one verdict per copy and per model
 outputs/classifier/                       scores (all data, per batch, per exercise type), figures
```

## Setup

```
pip install -r requirements.txt
```

A GPU is recommended for `classifier/03_train.ipynb`. To use it, install the CUDA build of `torch` and `torchvision` 
by following the command given on [pytorch.org](https://pytorch.org/get-started/locally/).

## Raw data

The `data/` folder is not versioned (images too large and sensitive), the notebooks create the folders they need. Put the scans in `data/raw/`, with one folder per student and one image per page.

There are two batches of scripts:
- **Batch 1**: 5 students, 15 pages, 27 exercises (q01 to q30), corrected in red by the teacher
- **Batch 2**: 10 students, 4 pages, 1 exercise per page (q31 to q34), not corrected

```
data/raw/
├── SET 1/                 ← batch 1
│   ├── 01.jpg             ← cover page
│   ├── 02.jpg
│   ├── ...
│   └── 15.jpg
├── SET 2/
├── ...
└── batch_2/
    ├── SET 1/
    │   ├── 01.jpg         ← exercise q31
    │   ├── ...
    │   └── 04.jpg         ← exercise q34
    ├── ...
    └── SET 10/
```

In the outputs, the students of batch 2 are named `b2_set_1` to `b2_set_10`, so that they are not mixed up with those of batch 1.

## Manual labeling

This is the only step that is not automated. After `01_cleaning.ipynb`, copy each image of `data/cleaned/by_exercise/qXX/` 
into `data/labeled/by_exercise/qXX/correct/` or `data/labeled/by_exercise/qXX/incorrect/`, depending on the teacher's correction. 
Keep the file names (`set_X_qXX.jpg`, `b2_set_X_qXX.jpg`), they tell which student and which exercise each image comes from.

## Folder structure

```
UTM/
├── data/                  ← not versioned (images too large and sensitive)
│   ├── raw/
│   ├── cleaned/
│   ├── labeled/
│   ├── processed/         ← approach 1
│   ├── templates/         ← approach 2, rebuilt blank worksheet of each exercise
│   ├── aligned/           ← approach 2, copies aligned on their template
│   └── marks/             ← approach 2, student marks only
├── notebooks/
│   ├── 01_cleaning.ipynb  ← shared by all approaches
│   ├── classifier/        ← approach 1, CNN on the whole image (02, 03)
│   └── reader/            ← approach 2, read the answer then compare with the answer key (02, ...)
├── config/
│   └── answer_key.csv     ← expected answer and answer type of each exercise
├── models/                ← trained models (not versioned)
├── outputs/
│   ├── predictions/       ← one file per approach, one row per copy
│   └── classifier/        ← scores and figures of approach 1
├── docs/                  ← literature review, CNN/LSTM comparison tables, data-cleaning (EN/FR)
│   └── classifier/        ← preprocessing and CNN training docs of approach 1 (EN/FR)
└── requirements.txt
```
