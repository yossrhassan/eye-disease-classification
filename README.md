# Eye Disease Classification Using Fundus Images

## Project Overview

This project develops a two-stage deep learning system for classifying eye diseases from fundus images.

The system is divided into two classification stages:

1. **Model 1:** Normal vs Abnormal classification.
2. **Model 2:** Classification of abnormal images into 7 disease classes.

The eight classes in the dataset are:

- Normal
- Diabetic Retinopathy
- Others
- Glaucoma
- Cataract
- Myopia
- AMD
- Hypertension

## Dataset

The dataset contains **11,839 fundus images** from multiple sources.

The unified class distribution is:

| Class | Images |
|---|---:|
| Normal | 4,698 |
| Diabetic Retinopathy | 4,113 |
| Others | 1,102 |
| Glaucoma | 930 |
| Cataract | 340 |
| Myopia | 294 |
| AMD | 274 |
| Hypertension | 88 |
| **Total** | **11,839** |

The dataset is highly imbalanced, particularly for the Hypertension and AMD classes.

The dataset is not included in this repository because of its large size.

## Data Preparation

Images were resized to **224 × 224** pixels.

Data augmentation was applied during training using:

- Random Rotation
- Random Zoom
- Random Translation

Class weights were used during model training to help address class imbalance.

The official validation split provided with the dataset was preserved. The train/test split was reconstructed while avoiding overlap with validation groups.

## Models

### Model 1 — Normal vs Abnormal

The first model performs binary classification:

- Normal
- Abnormal

All seven disease classes are grouped into the **Abnormal** category.

### Model 2 — Seven Disease Classes

Images classified as abnormal are passed to the second model, which predicts one of:

- Diabetic Retinopathy
- Others
- Glaucoma
- Cataract
- Myopia
- AMD
- Hypertension

### Custom CNN

A convolutional neural network was implemented from scratch as a baseline model.

### ResNet50

A pretrained **ResNet50** model with ImageNet weights was used for transfer learning.

The ResNet50 model was first evaluated with the convolutional base frozen. Fine-tuning was then performed on the seven-class classification task.

## Results

### Model Comparison

| Model | Task | Test Accuracy (%) | Macro F1 (%) |
|---|---|---:|---:|
| Custom CNN | Model 1 - Normal vs Abnormal | 68.00 | 65.00 |
| ResNet50 (Frozen) | Model 1 - Normal vs Abnormal | 80.35 | 79.00 |
| Custom CNN | Model 2 - 7 Disease Classes | 29.83 | 24.62 |
| ResNet50 (Frozen) | Model 2 - 7 Disease Classes | 52.49 | 45.00 |
| ResNet50 (Fine-Tuned) | Model 2 - 7 Disease Classes | 56.81 | 51.00 |
| End-to-End System | Model 1 → Model 2 | 58.00 | 48.00 |

## End-to-End System

The complete system follows this pipeline:

```text
Fundus Image
     │
     ▼
Model 1
Normal vs Abnormal
     │
     ├── Normal ──────────────► Normal
     │
     └── Abnormal
              │
              ▼
          Model 2
      7 Disease Classes
